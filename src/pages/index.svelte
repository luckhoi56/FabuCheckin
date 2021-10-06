<script>
  import { fade } from "svelte/transition";
  let m_modal = true;
  let m_chosenDate = new Date();
  let m_customer_appointment = {
    firstName: "",
    lastName: "",
    phoneNumbers: "",
    email: "",
    date: m_chosenDate,
    time: "9:00AM",
    service: "lash",
    technician: "Katie",
    raw_date: m_chosenDate
  };
  async function getServer(){
    const response = await fetch('https://burin-eyelash.herokuapp.com/')
    return await response.json()
  }

  async function sendAppointment() {
    m_customer_appointment.date = dateFormat(m_chosenDate)
    m_customer_appointment.raw_date = m_chosenDate
    if (formSanitize() == true) {
      m_confirmation_modal = true;
    } else {
      m_message_error_modal = true;
    }
    
    const res = await fetch("https://burin-eyelash.herokuapp.com/send-receipt", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(m_customer_appointment),
    });

    const json = await res.json();
    result = JSON.stringify(json);
    
   
  }
</script>
<!-- This example requires Tailwind CSS v2.0+ -->
<div class="bg-indigo-700">
  <div class="max-w-2xl mx-auto text-center py-16 px-4 sm:py-20 sm:px-6 lg:px-8">
    <h2 class="text-3xl font-extrabold text-white sm:text-4xl">
      <span class="block">Welcome to Fabu</span>
      <span class="block">Please check in and have a seat</span>
    </h2>
    
    <a href="#" class="mt-8 w-full inline-flex items-center justify-center px-5 py-3 border border-transparent text-base font-medium rounded-md text-indigo-600 bg-white hover:bg-indigo-50 sm:w-auto">
      Check In
    </a>
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
                  Booking Information
                </h3>
                <p class="mt-1 max-w-2xl text-sm text-gray-500">
                  Booking time 30 days in advance.
                </p>
              </div>
              <div class="space-y-6 sm:space-y-5">
                <div
                  class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5"
                >
                  <label
                    for="first_name"
                    class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2"
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
                      class="max-w-lg block w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:max-w-xs sm:text-sm border-gray-300 rounded-md"
                      required
                    />
                  </div>
                </div>

                <div
                  class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5"
                >
                  <label
                    for="last_name"
                    class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2"
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

                <div
                  class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5"
                >
                  <label
                    for="phoneNumber"
                    class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2"
                  >
                    Phone Numbers
                  </label>
                  <div class="mt-1 sm:mt-0 sm:col-span-2">
                    <input
                      type="tel"
                      placeholder="909-319-1234"
                      bind:value={m_customer_appointment["phoneNumbers"]}
                      id="phone"
                      name="phome"
                      autocomplete="phone"
                      class="block max-w-lg w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm border-gray-300 rounded-md"
                      required
                    />
                  </div>
                </div>

                <div
                  class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5"
                >
                  <label
                    for="email"
                    class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2"
                  >
                    Email address
                  </label>
                  <div class="mt-1 sm:mt-0 sm:col-span-2">
                    <input
                      bind:value={m_customer_appointment["email"]}
                      id="email"
                      name="email"
                      type="email"
                      autocomplete="email"
                      class="block max-w-lg w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm border-gray-300 rounded-md"
                      
                    />
                  </div>
                </div>

                

                <div
                  class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:border-t sm:border-gray-200 sm:pt-5"
                >
                  <label
                    for="times"
                    class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2"
                  >
                    Service
                  </label>
                  <div class="mt-1 sm:mt-0 sm:col-span-2">
                    <select
                      bind:value={m_customer_appointment["service"]}
                      id="service"
                      name="service"
                      autocomplete="service"
                      class="max-w-lg block focus:ring-indigo-500 focus:border-indigo-500 w-full shadow-sm sm:max-w-xs sm:text-sm border-gray-300 rounded-md"
                    >
                      <option>Lash</option>
                      <option>Microblading</option>
                      <option>Body Sculpting</option>
                    </select>
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
            on:click={sendAppointment}
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
              m_modal = false;
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
