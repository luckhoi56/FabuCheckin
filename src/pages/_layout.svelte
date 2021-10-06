<script>
  import { fade } from "svelte/transition";

  let current = "home";
  let m_flag_1 = "bloc";
  let m_flag_2 = "hidden";
  function toggle() {
    [m_flag_1, m_flag_2] = [m_flag_2, m_flag_1];
  }
  let m_highlight =
    "bg-yellow-50 border-yellow-500 text-yellow-700 block pl-3 pr-4 py-2 border-l-4 text-base font-medium";
  let m_non_highlight =
    "border-transparent text-gray-500 hover:bg-gray-50 hover:border-gray-300 hover:text-gray-700 block pl-3 pr-4 py-2 border-l-4 text-base font-medium";
</script>

<!-- This example requires Tailwind CSS v2.0+ -->
<nav class="bg-white shadow">
  <div class="max-w-7xl mx-auto px-2 sm:px-6 lg:px-8">
    <div class="relative flex justify-between h-16">
      <div class="absolute inset-y-0 left-0 flex items-center sm:hidden">
        <!-- Mobile menu button NEED MODIFY-->
        <button
          type="button"
          class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-yellow-500"
          aria-controls="mobile-menu"
          aria-expanded="false"
        >
          <span class="sr-only">Open main menu</span>
          <!--
            Icon when menu is closed.

            Heroicon name: outline/menu

            Menu open: "hidden", Menu closed: "block"
          -->
          <svg
            on:click={toggle}
            class="{m_flag_1} h-6 w-6"
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
              d="M4 6h16M4 12h16M4 18h16"
            />
          </svg>
          <!--
            Icon when menu is open.

            Heroicon name: outline/x

            Menu open: "block", Menu closed: "hidden"
          -->
          <svg
            on:click={toggle}
            class="{m_flag_2} h-6 w-6"
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
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </button>
      </div>
      <div
        class="flex-1 flex items-center justify-center sm:items-stretch sm:justify-start"
      >
        <div class="flex-shrink-0 flex items-center">
          <img
            class="block lg:hidden h-24 w-auto"
            src="https://burineyelash.s3.us-west-1.amazonaws.com/Images/logo2.PNG"
            alt="Workflow"
          />
          <img
            class="hidden lg:block h-24 w-25"
            src="https://burineyelash.s3.us-west-1.amazonaws.com/Images/logo2.PNG"
            alt="Workflow"
          />
        </div>
        <div class="hidden sm:ml-6 sm:flex sm:space-x-8">
          <!-- Current: "border-indigo-500 text-gray-900", Default: "border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700" -->
          <a
            href="/"
            class="{current === 'home'
              ? 'border-yellow-500'
              : ''} border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700 inline-flex items-center px-1 pt-1 border-b-2 text-sm font-medium"
            on:click={() => (current = "home")}
          >
            Home
          </a>
          <a
            href="/Staff"
            class=" {current === 'staff'
              ? 'border-yellow-500'
              : ''} border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700 inline-flex items-center px-1 pt-1 border-b-2 text-sm font-medium"
            on:click={() => (current = "staff")}
          >
            Staffs
          </a>

          <a
            href="/Gallary"
            class=" {current === 'about'
              ? 'border-yellow-500'
              : ''} border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700 inline-flex items-center px-1 pt-1 border-b-2 text-sm font-medium"
            on:click={() => (current = "about")}
          >
            Gallary
          </a>
          <a
            href="/Pricing"
            class=" {current === 'pricing'
              ? 'border-yellow-500'
              : ''} border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700 inline-flex items-center px-1 pt-1 border-b-2 text-sm font-medium"
            on:click={() => (current = "pricing")}
          >
            Pricing
          </a>
          <a
            href="/About"
            class=" {current === 'gallary'
              ? 'border-yellow-500'
              : ''} border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700 inline-flex items-center px-1 pt-1 border-b-2 text-sm font-medium"
            on:click={() => (current = "gallary")}
          >
            About Us
          </a>
        </div>
      </div>
      <div
        class="absolute inset-y-0 right-0 flex items-center pr-2 sm:static sm:inset-auto sm:ml-6 sm:pr-0"
      >
        <a href="tel:9495934776" target="_blank">
          <button
            class="bg-white p-1 rounded-full text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          >
            <span class="sr-only">View notifications</span>
            <!-- Heroicon name: outline/bell -->
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path
                d="M2 3a1 1 0 011-1h2.153a1 1 0 01.986.836l.74 4.435a1 1 0 01-.54 1.06l-1.548.773a11.037 11.037 0 006.105 6.105l.774-1.548a1 1 0 011.059-.54l4.435.74a1 1 0 01.836.986V17a1 1 0 01-1 1h-2C7.82 18 2 12.18 2 5V3z"
              />
            </svg>
          </button>
        </a>
        <!-- Profile dropdown -->
        <a href="https://goo.gl/maps/DjSRrCS3Xkj3fpe66" target="_blank">
          <button
            class="bg-white p-1 rounded-full text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          >
            <span class="sr-only">View notifications</span>
            <!-- Heroicon name: outline/bell -->
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"
              />
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"
              />
            </svg>
          </button>
        </a>
      </div>
    </div>
  </div>

  <!-- Mobile menu, show/hide based on menu state. -->
  <div class={m_flag_2} id="mobile-menu">
    <div class="pt-2 pb-4 space-y-1">
      <!-- Current: "bg-indigo-50 border-indigo-500 text-indigo-700", Default: "border-transparent text-gray-500 hover:bg-gray-50 hover:border-gray-300 hover:text-gray-700" -->

      <a
        on:click={() => (current = "home")}
        href="/"
        class={current === "home" ? m_highlight : m_non_highlight}>Home</a
      >
      <a
        on:click={() => (current = "staff")}
        href="/Staff"
        class={current === "staff" ? m_highlight : m_non_highlight}>Staffs</a
      >
      <a
        on:click={() => (current = "gallary")}
        href="/Gallary"
        class={current === "gallary" ? m_highlight : m_non_highlight}>Gallary</a
      >
      <a
        on:click={() => (current = "pricing")}
        href="/Pricing"
        class={current === "pricing" ? m_highlight : m_non_highlight}>Pricing</a
      >
      <a
        on:click={() => (current = "about")}
        href="/About"
        class={current === "about" ? m_highlight : m_non_highlight}>About</a
      >
    </div>
  </div>
</nav>

<slot />
