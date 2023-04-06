<script lang="ts">
  import Cloudinary from "./Cloudinary.svelte";
  import StepUpload from "./StepUpload.svelte";
  import StepEdit from "./StepEdit.svelte";
  import { imageStatus } from "./store";
  import { ImageStatus } from "./types.d";
</script>

<div
  class="max-w-xl m-auto grid grid-cols-1 place-content-center w-full h-screen p-4"
>
  <header class="flex justify-center py-10">
    <h1 class="text-3xl font-bold text-orange-300 tracking-tighter">
      <span class="text-blue-600">ImageTo</span>Sepia
    </h1>
  </header> 

  <main class="w-full block">
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </main>

  <footer class="flex justify-center items-center gap-x-2 font-semibold pt-10">
    Hecho con <a href="https://cloudinary" target="blank" rel="noreferrer"
      ><Cloudinary /></a
    >
  </footer>
</div>
