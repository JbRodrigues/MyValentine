<script>
    import { fade } from "svelte/transition";
    export let images = [];
    export let currentImage = 0;
    export let nextImage;
    export let prevImage;
    console.log("Imagens recebidas:", images);
    console.log("Imagem atual:", images[currentImage]);
</script>

<div
    class="relative mt-6 w-full max-w-2xl mx-auto aspect-[4/3] overflow-hidden rounded-lg shadow-lg"
>
    
    {#each images as image, index (index)}
        <img
            src={image}
            alt="Foto do casal"
            class="absolute top-0 left-0 w-full h-full object-contain transition-opacity duration-700 bg-pink-100"
            class:opacity-100={index === currentImage}
            class:opacity-0={index !== currentImage}
        />
    {/each}

    <button
        on:click={prevImage}
        aria-label="Imagem anterior"
        class="absolute left-2 top-1/2 transform -translate-y-1/2 bg-pink-500 text-white p-2 rounded-full z-10"
    >
        ←
    </button>
    <button
        on:click={nextImage}
        aria-label="Próxima imagem"
        class="absolute right-2 top-1/2 transform -translate-y-1/2 bg-pink-500 text-white p-2 rounded-full z-10"
    >
        →
    </button>
</div>

<style>
    .opacity-0 {
        opacity: 0;
    }
    .opacity-100 {
        opacity: 1;
    }
</style>
