<script lang="ts">
    import { originalImage, modifiedImage } from "./store";
    import "two-up-element";

    let processingImage = true
    let tries = 0
    let intervalId: any
    $: {
        if (processingImage) {
            clearInterval(intervalId)
            intervalId = setInterval(() => {
                tries++
            }, 500)
        }
    }
</script>

<two-up>
    <img src={$originalImage} alt="Imagen subida por el usuario" />
    <img
        on:load={() => processingImage = false}
        on:error={() => processingImage = true}
        src={`${$modifiedImage}&t=${tries}`} 
        alt="Imagen editada" 
    />
</two-up>
<div class="text-center w-full mt-8">
    <a
        download
        href={$modifiedImage}
        class="bg-blue-500 hover:bg-blue-700 text-xl font-bols text-white rounded-full px-4 py-2"
        >Descargar imagen editada</a
    >
</div>
