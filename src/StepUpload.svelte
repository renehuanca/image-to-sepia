<script lang="ts">
    import Dropzone from 'dropzone'
    import 'dropzone/dist/dropzone.css'
    import { Cloudinary } from '@cloudinary/url-gen'
    import { backgroundRemoval, sepia } from '@cloudinary/url-gen/actions/effect'
    import { ImageStatus } from './types.d'
    import { onMount } from 'svelte'
    import { imageStatus, modifiedImage, originalImage } from './store'

    const cloudinary = new Cloudinary({
        cloud: {
            cloudName: "dabvi9lbf",
        },
        url: {
            secure: true,
        },
    })

    onMount(() => {
        const dropzone = new Dropzone("#dropzone", {
            uploadMultiple: false,
            acceptedFiles: ".jpg,.png,.webp",
            maxFiles: 1,
        });

        dropzone.on("sending", (file, xhr, formData) => {
            imageStatus.set(ImageStatus.UPLOADING);
            formData.append("upload_preset", "uploadpr");
            formData.append("timestamp", Date.now() / 1000);
            formData.append("api_key", 711476323493477);
        });

        dropzone.on('success', (file, response) => {
            const { public_id: publicId, secure_url: url } = response;

            const imageWithoutBackground = cloudinary
                .image(publicId)
                // .effect(backgroundRemoval());
                .effect(sepia());

            imageStatus.set(ImageStatus.DONE);
            originalImage.set(url);
            modifiedImage.set(imageWithoutBackground.toURL());
            console.log("ha ido bien");
        });
        dropzone.on("error", (file, response) => {
            console.log("ha ido mal");
            console.log(response);
        });
    });
</script>

<form
    action="http://api.cloudinary.com/v1_1/dabvi9lbf/image/upload"
    id="dropzone"
    class="shadow-2xl border-dashed border-2 border-gray-300 rounded-lg aspect-video w-full flex justify-center items-center flex-col"
>
    {#if $imageStatus === ImageStatus.READY}
        <button
            class="font-bold pointer-events-none bg-blue-600 rounded-full text-bold text-white text-xl px-6 py-4"
            >Upload files</button
        >
        <strong class="text-lg mt-4 text-gray-800"> or drop a file</strong>
    {/if}

    {#if $imageStatus === ImageStatus.UPLOADING}
        <strong class="text-lg mt-4 text-gray-800"> Uploading file....</strong>
    {/if}
</form>
