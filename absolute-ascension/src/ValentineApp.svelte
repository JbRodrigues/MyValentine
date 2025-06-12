<script>
    import { onMount } from "svelte";

    // Componentes
    import Carousel from "./components/Carousel.svelte";
    import FallingHearts from "./components/FallingHearts.svelte";
    import TimerCounter from "./components/TimerCounter.svelte";
    import MusicPlayer from "./components/MusicPlayer.svelte";
    import coupleInfo from "./config.json";

    let couple = coupleInfo.couple;
    console.log(couple.name1, couple.name1);
    // Props simuladas (poderiam vir de API ou contexto)

    export let relationshipStartDate = "2024-03-16T21:00:00Z";

    let images = coupleInfo.images;

    // Carrossel
    let currentImage = 0;
    const nextImage = () => (currentImage = (currentImage + 1) % images.length);
    const prevImage = () =>
        (currentImage = (currentImage - 1 + images.length) % images.length);

    // Música
    let musicUrl = "/music/PearlJam.mp3"; // deve ser um arquivo .mp3 real
    let musicTitle = "Sirens"; // Título da música

    // Troca automática de imagem'
    let carouselInterval;

    onMount(() => {
        carouselInterval = setInterval(nextImage, 3000);
        return () => clearInterval(carouselInterval);
    });
</script>


<!-- Conteúdo principal -->
<div
    class="relative z-10 w-full min-h-screen bg-pink-100 flex flex-col items-center justify-center text-center px-4 py-8"
>
<!-- Corações caindo -->
<FallingHearts />
    <!-- Nomes do casal -->
    <h1 class="text-4xl font-bold text-pink-600 mb-4">
        {couple.name1} ❤️ {couple.name2}
    </h1>

    <MusicPlayer {musicUrl} {musicTitle} />

    <h2 class="text-2xl font-bold text-pink-600 mb-4 mt-2">
        Nossos momentos juntos...
    </h2>

    <!-- Carrossel -->
    {#if images.length}
        <Carousel {images} {currentImage} {nextImage} {prevImage} />
    {/if}

    <!-- Contador de tempo -->
    <TimerCounter {relationshipStartDate} />

    <!-- Declaração de amor -->
    <p class="mt-4 text-lg text-pink-700 italic text-center max-w-2xl">
        Dentre todas as definições possiveis de amor, eu escolho a que eu sinto por você.
        <br />
        Eu não sei exatamente se é um conjunto de sensações, sentimentos, emoções ou algo mais. Se é paixao, desejo, carinho, amizade ou companheirismo. Eu só sei que é você, e que só faz sentido porque é você.
        <br />
        Eu procurei encontrar em poemas e letras de musicas as definições que mais se aproximam, mas eu entendi que o que eu sinto não é simplesmente algo que possa ser descrito em palavras ou vibrações sonoras.
        <br />
        Eu só sei que o que eu sinto, eu escolhi sentir, mesmo que eu não tenha controle nenhum sobre isso.
        <br />
        Esse é o segundo dia dos namorados que passamos juntos e eu só queria dizer que, eu só vou parar de te amar quando esse contador de segundo ai em cima chegar em 61.
        <br />
        Beijos, EU AMO VOCÊ!
        <br />
        Do seu e só seu, Julio.
    </p>
</div>
