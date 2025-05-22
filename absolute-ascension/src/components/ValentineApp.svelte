<script>
    export let couple;
    export let relationshipStartDate;
    export let images = [];

    import { onMount } from "svelte";
    import { writable } from "svelte/store";

    // Carrossel
    let currentImage = 0;
    const nextImage = () => (currentImage = (currentImage + 1) % images.length);
    const prevImage = () =>
        (currentImage = (currentImage - 1 + images.length) % images.length);

    // Contador de tempo
    const startDate = new Date(relationshipStartDate);

    const calculateTimeTogether = () => {
        const now = new Date();
        const diff = now - startDate;

        const yearMs = 1000 * 60 * 60 * 24 * 365.25;
        const monthMs = yearMs / 12;
        const dayMs = 1000 * 60 * 60 * 24;

        const years = Math.floor(diff / yearMs);
        const months = Math.floor((diff % yearMs) / monthMs);
        const days = Math.floor((diff % monthMs) / dayMs);

        return { years, months, days };
    };

    const timeTogether = writable(calculateTimeTogether());

    // Atualiza diariamente
    onMount(() => {
        const interval = setInterval(
            () => {
                timeTogether.set(calculateTimeTogether());
            },
            1000 * 60 * 60 * 24,
        ); // 1x ao dia

        return () => clearInterval(interval);
    });

    // Corações caindo
    let hearts = [];

    onMount(() => {
        const createHeart = () => {
            const heart = {
                id: crypto.randomUUID?.() || Math.random(),
                left: Math.random() * 100,
                animationDuration: 3 + Math.random() * 2,
            };
            hearts = [...hearts, heart];

            setTimeout(() => {
                hearts = hearts.filter((h) => h.id !== heart.id);
            }, heart.animationDuration * 1000);
        };

        const interval = setInterval(createHeart, 500);
        return () => clearInterval(interval);
    });

    const heartStyle = (heart) =>
        `left: ${heart.left}%; animation: fall ${heart.animationDuration}s linear;`;
</script>

<div class="relative w-full max-w-2xl mx-auto text-center">
    <!-- Nomes do Casal -->
    <h1 class="text-4xl font-bold text-pink-600 mb-4">
        {couple.name1} ❤️ {couple.name2}
    </h1>

    <!-- Contador -->
    {#each Object.entries($timeTogether) as [unit, value]}
        {#if value > 0}
            <p class="text-xl text-pink-800">
                {value}
                {unit === "years"
                    ? " ano(s)"
                    : unit === "months"
                      ? " mês(es)"
                      : " dia(s)"}
            </p>
        {/if}
    {/each}

    <!-- Carrossel -->
    {#if images.length}
        <div class="relative mt-6 overflow-hidden rounded-lg shadow-lg">
            <img
                src={images[currentImage]}
                alt="Foto do casal"
                class="w-full h-64 object-cover"
            />
            <button
                on:click={prevImage}
                class="absolute left-2 top-1/2 transform -translate-y-1/2 bg-pink-500 text-white p-2 rounded-full"
            >
                ←
            </button>
            <button
                on:click={nextImage}
                class="absolute right-2 top-1/2 transform -translate-y-1/2 bg-pink-500 text-white p-2 rounded-full"
            >
                →
            </button>
        </div>
    {/if}

    <!-- Corações caindo -->
    {#each hearts as heart (heart.id)}
        <div class="falling-heart" style={heartStyle(heart)}>❤️</div>
    {/each}
</div>

<style>
    @keyframes fall {
        0% {
            transform: translateY(-100px);
            opacity: 1;
        }
        100% {
            transform: translateY(100vh);
            opacity: 0;
        }
    }

    .falling-heart {
        position: fixed;
        top: 0;
        z-index: 50;
        animation-timing-function: linear;
        pointer-events: none;
    }
</style>
