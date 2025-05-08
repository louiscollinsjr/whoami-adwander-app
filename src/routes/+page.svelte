<!-- <h1 class="text-3xl font-bold underline font-roboto p-4">Welcome to SvelteKit</h1>
<p class="text-xs font-roboto">Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p> -->

<script context="module">
    export let title = "Your IP Address | ip.adwander.com";
    export let description = "Instant IP address lookup tool. Find your public IP, location, and more in one click.";
  </script>
  
  <svelte:head>
    <!-- SEO Meta tags -->
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="{description}" />
    <title>{title}</title>
  
    <!-- Preload fonts -->
    <link rel="preload" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" as="font" type="font/woff2" crossorigin="anonymous" />
  
    <!-- Lazy-load Google Ads -->
    <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
  </svelte:head>
  
  <script>
    import { onMount } from "svelte";
    import Clipboard from "phosphor-svelte/lib/Clipboard";
    import Copy from "phosphor-svelte/lib/Copy";
    import { toast } from "svelte-sonner";
    import { Button } from "$lib/components/ui/button";
   
  
    let ip = "Loading...";
    let locationInfo = "";
  
    onMount(async () => {
      const response = await fetch("https://ipwho.is/");
      const data = await response.json();
      ip = data.ip;
      locationInfo = `
        <div class="space-y-2 text-center font-jetbrains text-xs">
          <div class="flex justify-center gap-2">
            <span class="font-semibold">Location:</span>
            <span>${data.city}, ${data.region}, ${data.country}</span>
          </div>
          <div class="flex justify-center gap-2">
            <span class="font-semibold">Timezone:</span>
            <span>${data.timezone?.id}</span>
          </div>
          <div class="flex justify-center gap-2">
            <span class="font-semibold">ISP:</span>
            <span>${data.connection?.isp || 'N/A'}</span>
          </div>
        </div>
      `;
    });
  
    function copyIP() {
      navigator.clipboard.writeText(ip);
      toast.success("IP copied to clipboard!", {
        description: `Copied: ${ip}`
      });
    }
  </script>
  
  <main class="w-full flex flex-col min-h-screen">
    <div class="flex-grow flex flex-col items-center justify-center px-4">
      <div class="text-center w-full max-w-md sm:max-w-lg md:max-w-xl mx-auto">
        <h1 class="text-2xl sm:text-3xl font-bold font-geist-sans my-8">Your IP Address</h1>

        <div id="ip" class="text-2xl sm:text-4xl font-bold mt-4 p-4 bg-white rounded-lg shadow-sm relative mx-auto w-[90%] sm:w-[80%]">
          <span class="block">{ip}</span>
          <button
            class="absolute right-4 top-1/2 -translate-y-1/2 p-2 hover:bg-gray-100 rounded-lg transition-colors"
            on:click={copyIP}
          >
            <Copy size={16} weight="bold" class="text-gray-300 hover:text-gray-700" />
          </button>
        </div>

        <Button
          class="mt-6 text-xs font-bold mx-auto"
          on:click={copyIP}
        >
          Copy to Clipboard
        </Button>

        <div class="mt-4 text-gray-700 font-roboto p-y-4">
          {#if locationInfo}
            {@html locationInfo}
          {/if}
        </div>

      

        <!-- Lazy-load Ad here -->
        <div class="mt-6">
          <ins class="adsbygoogle"
               style="display:block"
               data-ad-client="ca-pub-xxxxxxxxxxxxxxx"
               data-ad-slot="1234567890"
               data-ad-format="auto"></ins>
          <script>
            (adsbygoogle = window.adsbygoogle || []).push({});
          </script>
        </div>
      </div>
    </div>
    <footer class="w-full text-center py-4 text-xs text-gray-500 opacity-80 font-geist-sans">
      &copy; 2025 adwander.com — Built with privacy in mind, supported by ads.
    </footer>
  </main>
