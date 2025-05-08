<script lang="ts">
  import { i18n } from '$lib/i18n';
  import { ParaglideJS } from '@inlang/paraglide-sveltekit';
  import '../app.css';
  import { Toaster } from '$lib/components/ui/sonner';
  let { children } = $props();
</script>


<div class="min-h-screen bg-gray-100 flex items-center justify-center">
  <Toaster />
  <ParaglideJS {i18n}>
    {@render children()}
  </ParaglideJS>
</div>
