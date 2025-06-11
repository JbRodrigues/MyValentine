<script>
    import { onMount } from "svelte";
    

    // Componentes
    import Carousel from "./components/Carousel.svelte";
    import FallingHearts from "./components/FallingHearts.svelte";
    import TimerCounter from "./components/TimerCounter.svelte";
    import MusicPlayer from "./components/MusicPlayer.svelte";

    // Props simuladas (poderiam vir de API ou contexto)
    export let couple = {
        name1: "Maria",
        name2: "João",
    };

    export let relationshipStartDate = "2024-03-16T21:00:00Z";

    export let images = ["/img/foto1.jpg", "/img/foto2.jpg", "/img/foto3.jpg"];

    // Carrossel
    let currentImage = 0;
    const nextImage = () => (currentImage = (currentImage + 1) % images.length);
    const prevImage = () =>
        (currentImage = (currentImage - 1 + images.length) % images.length);

    // Música
    let musicUrl = "/musica.mp3"; // deve ser um arquivo .mp3 real
    let musicTitle = "Nossa Canção Especial";

    // Troca automática de imagem
    let carouselInterval;

    onMount(() => {
        carouselInterval = setInterval(nextImage, 3000);
        return () => clearInterval(carouselInterval);
    });
</script>

<!-- Corações caindo -->
<FallingHearts />

<!-- Conteúdo principal -->
<div
    class="relative z-10 w-full min-h-screen bg-pink-100 flex flex-col items-center justify-center text-center px-4 py-8"
>
    <!-- Nomes do casal -->
    <h1 class="text-4xl font-bold text-pink-600 mb-4">
        {couple.name1} ❤️ {couple.name2}
    </h1>

    <!-- Carrossel -->
    {#if images.length}
        <Carousel {images} {currentImage} {nextImage} {prevImage} />
    {/if}

    <!-- Contador de tempo -->
    <TimerCounter {relationshipStartDate} />

    <!-- Declaração de amor -->
    <p class="mt-4 text-lg text-pink-700 italic">
        Que cada dia ao seu lado seja mais um capítulo lindo da nossa história.
        Te amo infinitamente!
    </p>

    <!-- Player de música -->
    <MusicPlayer {musicUrl} {musicTitle} />
</div>
