<script>
  import { fade } from "svelte/transition";
  import spacetime from 'spacetime'
  //const oneYear = 1000 * 60 * 60 * 24 * 365; //
  
  let m_customer_found = false;
  let m_customer_not_found = false
  var customer = ''
  const oneMonth = 1000 * 60 * 60 * 24 * 30;
  const oneDay = 1000 * 60 * 60 * 24
  let m_modal = false;

  let minDate = new Date(Date.now() + oneDay ); // appoinment is only available for tmr
  let maxDate = new Date(Date.now()  + oneMonth);
  let m_chosenDate = new Date();
  let m_message_error = "";
  let m_message_error_modal = false;
  let m_customer_update = false
  let m_blank_customer_appoinment = {
    firstName: "",
    lastName: "",
    phoneNumbers: "",
    email: "",
    date: m_chosenDate,
    time: "",
    service: "",
    technician: "",
    type:'checkin',
    status:'n/a'
  };
  let m_customer_appointment = {
    firstName: "",
    lastName: "",
    phoneNumbers: "",
    email: "",
    date: m_chosenDate,
    time: "",
    service: "",
    technician: "",
    type:'checkin',
    status:'n/a'
  };
  let m_confirmation_modal = false;
  function dateFormat(dateObject){
    //let weekDay = new Intl.DateTimeFormat('en', { weekday: 'long' }).format(dateObject);
    let ye = new Intl.DateTimeFormat('en', { year: 'numeric' }).format(dateObject);
    let mo = new Intl.DateTimeFormat('en', { month: 'short' }).format(dateObject);
    let da = new Intl.DateTimeFormat('en', { day: '2-digit' }).format(dateObject);
    //return (`${weekDay} ${da}-${mo}-${ye}`);
    return (`${da}-${mo}-${ye}`);

  }
  async function getServer(){
    const response = await fetch('https://burin-eyelash.herokuapp.com/')
    return await response.json()
  }

  async function sendCheckin() {
    if(formSanitizePhone()==false){
      m_message_error_modal = true;
      return
    }
   
    const res = await fetch("https://burin-eyelash.herokuapp.com/find-customer", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(m_customer_appointment),
    });

    const json = await res.json();
    let result = json
    console.log(result)
    if(result == 'not found')
    {
      m_customer_not_found = true
    }
    else{
      m_modal = false
      m_customer_found = true
      customer = result
      m_customer_appointment.firstName = result.firstName
      m_customer_appointment.lastName = result.lastName
      console.log(typeof(customer.firstName))
    }
  }

  async function sendCheckinUpdate() {
    if(formSanitizeName()==false){
      console.log('aha')
      m_message_error_modal = true;
      return false
    }
    let s =spacetime.now('America/Los_Angeles')
    m_customer_appointment.date = `${s.date()}-${s.monthName()}-${s.year()}`
    m_customer_appointment.time = `${s.time()}`
    console.log('cool')
    const res = await fetch("https://burin-eyelash.herokuapp.com/update-customer", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(m_customer_appointment),
    });
    
    m_confirmation_modal=true
    m_customer_update = m_customer_found = m_customer_not_found=false
    const json = await res.json();
    m_customer_appointment = {...m_blank_customer_appoinment}
    
    
  }
  function formSanitizePhone() {
    if(/^(1\s|1|)?((\(\d{3}\))|\d{3})(\-|\s)?(\d{3})(\-|\s)?(\d{4})$/.test(m_customer_appointment["phoneNumbers"]) == false ){
      m_message_error = "not a valid phone numbers"
      return false
    }
    return true
  }
  function formSanitizeName() {
    
    if(m_customer_appointment['firstName'] == ''){
      m_message_error = "first name is invalid"
      return false
    }
    if(m_customer_appointment['lastName'] == ''){
      m_message_error = "last name is invalid"
      return false
    }
      
    else return true;
  }
</script>

<!-- This example requires Tailwind CSS v2.0+ -->

<div class="relative bg-white overflow-hidden">
  <div class="max-w-7xl mx-auto">
    <div
      class="relative z-10 pb-8 bg-white sm:pb-16 md:pb-20 lg:max-w-2xl lg:w-full lg:pb-28 xl:pb-32"
    >
      <svg
        class="hidden lg:block absolute right-0 inset-y-0 h-full w-48 text-white transform translate-x-1/2"
        fill="currentColor"
        viewBox="0 0 100 100"
        preserveAspectRatio="none"
        aria-hidden="true"
      >
        <polygon points="50,0 100,0 50,100 0,100" />
      </svg>

      <!--
        Mobile menu, show/hide based on menu open state.

        Entering: "duration-150 ease-out"
          From: "opacity-0 scale-95"
          To: "opacity-100 scale-100"
        Leaving: "duration-100 ease-in"
          From: "opacity-100 scale-100"
          To: "opacity-0 scale-95"
      -->

      <main
        class="mt-10 mx-auto max-w-7xl px-4 sm:mt-12 sm:px-6 md:mt-16 lg:mt-20 lg:px-8 xl:mt-28"
      >
        <div class="sm:text-center lg:text-left">
          <h1
            class="text-4xl tracking-tight font-extrabold text-gray-900 sm:text-5xl md:text-6xl"
          >
            <span class="block xl:inline">Take a tour to</span>
            <span class="block xl:inline" style="color:#BE8664"
              >Fabu Lash & Body Spa</span
            >
          </h1>
          <p
            class="mt-3 text-base text-gray-500 sm:mt-5 sm:text-lg sm:max-w-xl sm:mx-auto md:mt-5 md:text-xl lg:mx-0"
          />
          <div>
            <strong>Business Hours:</strong>
            <br>
            Monday to Saturday: 9AM - 7:30PM
            <br>
            Sunday: 10AM - 6PM
          </div>
          <br>
          <div>
            <strong>Address:</strong>
            <br>
            27311 La Paz Rd, Ste D, Laguna Niguel, CA 92677
          </div>
          <div class="mt-5 sm:mt-8 sm:flex sm:justify-center lg:justify-start">
            <div class="rounded-md shadow">
              <a
                on:click={() => (m_modal = true)}
                href="#"
                class="w-full flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-md text-white  bg-yellow-800 hover:bg-yellow-700 md:py-4 md:text-lg md:px-10" 
              >
                Check In
              </a>
            </div>
            
          </div>
        </div>
      </main>
    </div>
  </div>
  <div transition:fade class="lg:absolute lg:inset-y-0 lg:right-0 lg:w-1/2">
    <img
      class="h-56 w-full object-cover sm:h-72 md:h-96 lg:w-full lg:h-full"
      src="https://burineyelash.s3-us-west-1.amazonaws.com/Images/eyelashModel.jpg"
      alt=""
    />
  </div>
</div>

{#if m_modal === true}
  <!-- Modal -->
  <div
    transition:fade
    class="fixed z-10 inset-0 overflow-y-auto"
    aria-labelledby="modal-title"
    role="dialog"
    aria-modal="true"
  >
    <div
      class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0"
    >
      <!--
      Background overlay, show/hide based on modal state.

      Entering: "ease-out duration-300"
        From: "opacity-0"
        To: "opacity-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100"
        To: "opacity-0"
    -->
      <div
        class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
        aria-hidden="true"
      />

      <!-- This element is to trick the browser into centering the modal contents. -->
      <span
        class="hidden sm:inline-block sm:align-middle sm:h-screen"
        aria-hidden="true">&#8203;</span
      >

      <!--
      Modal panel, show/hide based on modal state.

      Entering: "ease-out duration-300"
        From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
        To: "opacity-100 translate-y-0 sm:scale-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100 translate-y-0 sm:scale-100"
        To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
    -->
      <div
        class="inline-block align-bottom bg-white rounded-lg px-4 pt-5 pb-4 text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full sm:p-6"
      >
        <!--

       <!--
  This example requires Tailwind CSS v2.0+ 
  
  This example requires some changes to your config:
  
  ```
  // tailwind.config.js
  module.exports = {
    // ...
    plugins: [
      // ...
      require('@tailwindcss/forms'),
    ]
  }
  ```
-->
        <form class="space-y-8 divide-y divide-gray-200">
          <div class="space-y-8 divide-y divide-gray-200 sm:space-y-5">
            <div class="pt-8 space-y-6 sm:pt-10 sm:space-y-5">
              <div>
                <h3 class="text-lg leading-6 font-medium text-gray-900">
                  Checkin Information
                </h3>
                <br>
              
             


                <div
                  class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5"
                >
                  <label
                    for="phoneNumber"
                    class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2 required"
                  >
                    Phone Numbers
                  </label>
                  <div class="mt-1 sm:mt-0 sm:col-span-2">
                    <input
                      type="tel"
                      placeholder="9093191234"
                      bind:value={m_customer_appointment["phoneNumbers"]}
                      id="phone"
                      name="phome"
                      autocomplete="phone"
                      class="  block max-w-lg w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm border-gray-300 rounded-md"
                      required
                    />
                  </div>
                </div>

                

               

               


               


                
              </div>
            </div>
          </div>
        </form>

        <div
          class="mt-5 sm:mt-6 sm:grid sm:grid-cols-2 sm:gap-3 sm:grid-flow-row-dense"
        > 
          {#await getServer()}
            <button
            
            type="button"
            class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-yellow-700 text-base font-medium text-white hover:bg-yellow-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500 sm:col-start-2 sm:text-sm"
          >
            Waiting
          </button>
          {:then data}
             <button
            on:click={sendCheckin}
            type="button"
            class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-yellow-700 text-base font-medium text-white hover:bg-yellow-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500 sm:col-start-2 sm:text-sm"
          >
            Confirm
          </button>
          {:catch error}
             <button
           
            type="button"
            class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-yellow-700 text-base font-medium text-white hover:bg-yellow-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500 sm:col-start-2 sm:text-sm"
          >
            Error: Cannot GET
          </button>
          {/await}
         
          <button
            on:click={() => {
              m_customer_found = m_customer_not_found=  false
              m_modal = false;
              m_customer_appointment = {...m_blank_customer_appoinment}
            }}
            type="button"
            class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500 sm:mt-0 sm:col-start-1 sm:text-sm"
          >
            Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
{/if}

{#if (m_customer_found == true)}
  <!-- This example requires Tailwind CSS v2.0+ -->
  <div
    transition:fade
    class="fixed z-10 inset-0 overflow-y-auto"
    aria-labelledby="modal-title"
    role="dialog"
    aria-modal="true"
  >
    <div
      class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0"
    >
      <!--
      Background overlay, show/hide based on modal state.

      Entering: "ease-out duration-300"
        From: "opacity-0"
        To: "opacity-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100"
        To: "opacity-0"
    -->
      <div
        class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
        aria-hidden="true"
      />

      <!-- This element is to trick the browser into centering the modal contents. -->
      <span
        class="hidden sm:inline-block sm:align-middle sm:h-screen"
        aria-hidden="true">&#8203;</span
      >

      <!--
      Modal panel, show/hide based on modal state.

      Entering: "ease-out duration-300"
        From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
        To: "opacity-100 translate-y-0 sm:scale-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100 translate-y-0 sm:scale-100"
        To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
    -->

      <div
        class="inline-block align-bottom bg-white rounded-lg px-4 pt-5 pb-4 text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-sm sm:w-full sm:p-6"
      >
        <div>
          <div
            class="mx-auto flex items-center justify-center h-12 w-12 rounded-full bg-green-100"
          >
            <!-- Heroicon name: outline/check -->
            <svg
              class="h-6 w-6 text-green-600"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              aria-hidden="true"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M5 13l4 4L19 7"
              />
            </svg>
          </div>
          
          <div class="mt-3 text-center sm:mt-5">
            <h3
              class="text-lg leading-6 font-medium text-gray-900"
              id="modal-title"
            >
              Welcome back
            </h3>
            <h3
              class="text-lg leading-6 font-medium text-gray-900"
              id="modal-title"
            >
              {customer.firstName} {customer.lastName}
            </h3>
            
          </div>
        </div>
        <div class="float-right justify-center mt-6 grid grid-cols-2 gap-5">
          <button 
          on:click={() => {
            m_customer_update = true;
          }}
          type="button" class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
            Update
          </button>

          

          <button 
          on:click = {async () =>{
            m_confirmation_modal = true
            m_customer_update = m_customer_found= false
            let s =spacetime.now('America/Los_Angeles')
            m_customer_appointment.date = `${s.month()+1}/${s.date()}/${s.year()}`
            m_customer_appointment.time = `${s.time()}`
            const res = await fetch("https://burin-eyelash.herokuapp.com/update-customer", {
              method: "POST",
              headers: {
                "Content-Type": "application/json",
              },
              body: JSON.stringify(m_customer_appointment),
            });
            m_customer_appointment ={...m_blank_customer_appoinment}
          }}
          type="button" class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
            Check In 
          </button>
        </div>
      </div>
    </div>
  </div>
{/if}






{#if (m_customer_update === true ||  m_customer_not_found == true)}
  <!-- Modal -->
  <div
    transition:fade
    class="fixed z-10 inset-0 overflow-y-auto"
    aria-labelledby="modal-title"
    role="dialog"
    aria-modal="true"
  >
    <div
      class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0"
    >
      <!--
      Background overlay, show/hide based on modal state.

      Entering: "ease-out duration-300"
        From: "opacity-0"
        To: "opacity-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100"
        To: "opacity-0"
    -->
      <div
        class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
        aria-hidden="true"
      />

      <!-- This element is to trick the browser into centering the modal contents. -->
      <span
        class="hidden sm:inline-block sm:align-middle sm:h-screen"
        aria-hidden="true">&#8203;</span
      >

      <!--
      Modal panel, show/hide based on modal state.

      Entering: "ease-out duration-300"
        From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
        To: "opacity-100 translate-y-0 sm:scale-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100 translate-y-0 sm:scale-100"
        To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
    -->
      <div
        class="inline-block align-bottom bg-white rounded-lg px-4 pt-5 pb-4 text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full sm:p-6"
      >
        <!--

       <!--
  This example requires Tailwind CSS v2.0+ 
  
  This example requires some changes to your config:
  
  ```
  // tailwind.config.js
  module.exports = {
    // ...
    plugins: [
      // ...
      require('@tailwindcss/forms'),
    ]
  }
  ```
-->
        <form class="space-y-8 divide-y divide-gray-200">
          <div class="space-y-8 divide-y divide-gray-200 sm:space-y-5">
            <div class="pt-8 space-y-6 sm:pt-10 sm:space-y-5">
              <div>
                <h3 class="text-lg leading-6 font-medium text-gray-900">
                  Checkin Information Update
                </h3>
                <br>
              
             


                <div class="space-y-6 sm:space-y-5">
                  <div
                    class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5"
                  >
                    <label
                      for="first_name"
                      class="required block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2"
                    >
                      First name
                    </label>
                    <div class="mt-1 sm:mt-0 sm:col-span-2">
                      <input
                        bind:value={m_customer_appointment["firstName"]}
                        type="text"
                        name="first_name"
                        id="first_name"
                        autocomplete="given-name"
                        class=" max-w-lg block w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:max-w-xs sm:text-sm border-gray-300 rounded-md"
                        required
                      />
                    </div>
                  </div>
  
                  <div
                    class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5"
                  >
                    <label
                      for="last_name"
                      class="required block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2"
                    >
                      Last name
                    </label>
                    <div class="mt-1 sm:mt-0 sm:col-span-2">
                      <input
                        bind:value={m_customer_appointment["lastName"]}
                        type="text"
                        name="last_name"
                        id="last_name"
                        autocomplete="family-name"
                        class="max-w-lg block w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:max-w-xs sm:text-sm border-gray-300 rounded-md"
                        required
                      />
                    </div>
                  </div>
  
                
              </div>
            </div>
          </div>
        </form>

        <div
          class="mt-5 sm:mt-6 sm:grid sm:grid-cols-2 sm:gap-3 sm:grid-flow-row-dense"
        > 
          {#await getServer()}
            <button
            
            type="button"
            class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-yellow-700 text-base font-medium text-white hover:bg-yellow-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500 sm:col-start-2 sm:text-sm"
          >
            Waiting
          </button>
          {:then data}
             <button
            on:click={sendCheckinUpdate}
            type="button"
            class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-yellow-700 text-base font-medium text-white hover:bg-yellow-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500 sm:col-start-2 sm:text-sm"
          >
            Confirm
          </button>
          {:catch error}
             <button
           
            type="button"
            class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-yellow-700 text-base font-medium text-white hover:bg-yellow-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500 sm:col-start-2 sm:text-sm"
          >
            Error: Cannot GET
          </button>
          {/await}
         
          <button
            on:click={() => {
              m_modal = m_customer_update = m_customer_found = m_customer_not_found=false;
              console.log(m_customer_update)
              m_customer_appointment = {...m_blank_customer_appoinment}
            }}
            type="button"
            class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500 sm:mt-0 sm:col-start-1 sm:text-sm"
          >
            Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
{/if}


{#if m_message_error_modal == true}
  <!-- This example requires Tailwind CSS v2.0+ -->
  <div
    transition:fade
    class="fixed z-10 inset-0 overflow-y-auto"
    aria-labelledby="modal-title"
    role="dialog"
    aria-modal="true"
  >
    <div
      class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0"
    >
      <!--
      Background overlay, show/hide based on modal state.

      Entering: "ease-out duration-300"
        From: "opacity-0"
        To: "opacity-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100"
        To: "opacity-0"
    -->
      <div
        class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
        aria-hidden="true"
      />

      <!-- This element is to trick the browser into centering the modal contents. -->
      <span
        class="hidden sm:inline-block sm:align-middle sm:h-screen"
        aria-hidden="true">&#8203;</span
      >

      <!--
      Modal panel, show/hide based on modal state.

      Entering: "ease-out duration-300"
        From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
        To: "opacity-100 translate-y-0 sm:scale-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100 translate-y-0 sm:scale-100"
        To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
    -->
      <div
        class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full"
      >
        <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
          <div class="sm:flex sm:items-start">
            <div
              class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10"
            >
              <!-- Heroicon name: outline/exclamation -->
              <svg
                class="h-6 w-6 text-red-600"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                aria-hidden="true"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"
                />
              </svg>
            </div>
            <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
              <h3
                class="text-lg leading-6 font-medium text-gray-900"
                id="modal-title"
              >
                Invalid
              </h3>
              <div class="mt-2">
                <p class="text-sm text-gray-500">
                  {m_message_error}
                </p>
              </div>
            </div>
          </div>
        </div>
        <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
          <button
            on:click={() => {
              m_message_error_modal = false;
            }}
            type="button"
            class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-red-600 text-base font-medium text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 sm:ml-3 sm:w-auto sm:text-sm"
          >
            return
          </button>
        </div>
      </div>
    </div>
  </div>
{/if}
{#if m_confirmation_modal == true}
  <!-- This example requires Tailwind CSS v2.0+ -->
  <div
    transition:fade
    class="fixed z-10 inset-0 overflow-y-auto"
    aria-labelledby="modal-title"
    role="dialog"
    aria-modal="true"
  >
    <div
      class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0"
    >
      <!--
      Background overlay, show/hide based on modal state.

      Entering: "ease-out duration-300"
        From: "opacity-0"
        To: "opacity-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100"
        To: "opacity-0"
    -->
      <div
        class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
        aria-hidden="true"
      />

      <!-- This element is to trick the browser into centering the modal contents. -->
      <span
        class="hidden sm:inline-block sm:align-middle sm:h-screen"
        aria-hidden="true">&#8203;</span
      >

      <!--
      Modal panel, show/hide based on modal state.

      Entering: "ease-out duration-300"
        From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
        To: "opacity-100 translate-y-0 sm:scale-100"
      Leaving: "ease-in duration-200"
        From: "opacity-100 translate-y-0 sm:scale-100"
        To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
    -->

      <div
        class="inline-block align-bottom bg-white rounded-lg px-4 pt-5 pb-4 text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-sm sm:w-full sm:p-6"
      >
        <div>
          <div
            class="mx-auto flex items-center justify-center h-12 w-12 rounded-full bg-green-100"
          >
            <!-- Heroicon name: outline/check -->
            <svg
              class="h-6 w-6 text-green-600"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              aria-hidden="true"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M5 13l4 4L19 7"
              />
            </svg>
          </div>

          <div class="mt-3 text-center sm:mt-5">
            <h3
              class="text-lg leading-6 font-medium text-gray-900"
              id="modal-title"
            >
              Check In Successfully!
            </h3>
            <div class="mt-2">
              <p class="text-sm text-gray-500">
                Have a seat
              </p>
            </div>
          </div>
        </div>
        <div class="mt-5 sm:mt-6">
          <button
            on:click={() => {
              m_modal = m_confirmation_modal = false;
            }}
            type="button"
            class="inline-flex justify-center w-full rounded-md border border-transparent shadow-sm px-4 py-2 bg-yellow-700 text-base font-medium text-white hover:bg-yellow-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:text-sm"
          >
            Go back to homepage
          </button>
          
        </div>
      </div>
    </div>
  </div>
{/if}
<style>
  .required:after {
    content:" *";
    color: red;
  }
</style>