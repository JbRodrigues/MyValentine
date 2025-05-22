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
        const hourMs = 1000 * 60 * 60;
        const minuteMs = 1000 * 60;
        const secondMs = 1000;

        const years = Math.floor(diff / yearMs);
        const months = Math.floor((diff % yearMs) / monthMs);
        const days = Math.floor((diff % monthMs) / dayMs);
        const hours = Math.floor((diff % dayMs) / hourMs);
        const minutes = Math.floor((diff % hourMs) / minuteMs);
        const seconds = Math.floor((diff % minuteMs) / secondMs);

        return { years, months, days, hours, minutes, seconds };
    };

    const timeTogether = writable(calculateTimeTogether());

    // Atualiza diariamente
    onMount(() => {
        const interval = setInterval(() => {
            timeTogether.set(calculateTimeTogether());
        }, 1000); // Atualiza a cada segundo

        return () => clearInterval(interval);
    });

    // Corações caindo
    let hearts = [];

    onMount(() => {
        const createHeart = () => {
            const heart = {
                id: crypto.randomUUID?.() || Math.random(),
                left: Math.random() * 100,
                size: 16 + Math.random() * 24,
                animationDuration: 3 + Math.random() * 2,
            };
            hearts = [...hearts, heart];

            setTimeout(() => {
                hearts = hearts.filter((h) => h.id !== heart.id);
            }, heart.animationDuration * 1000);
        };

        const interval = setInterval(createHeart, 400);
        return () => clearInterval(interval);
    });

    const heartStyle = (heart) =>
        `left: ${heart.left}vw; font-size: ${heart.size}px; animation: fall ${heart.animationDuration}s linear forwards;`;
</script>

<div class="relative w-full max-w-2xl mx-auto text-center">
    <!-- Nomes do Casal -->
    <h1 class="text-4xl font-bold text-pink-600 mb-4">
        {couple.name1} ❤️ {couple.name2}
    </h1>

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

    <h2 class="text-2xl font-bold text-pink-600 mb-4">
        Eu te amo a exatamente
    </h2>

    <!-- Contador -->
    {#each Object.entries($timeTogether) as [unit, value]}
        {#if value > 0}
            <p class="text-xl text-pink-800">
                {value}
                {unit === "years"
                    ? " ano(s)"
                    : unit === "months"
                      ? " mês(es)"
                      : unit === "days"
                        ? " dia(s)"
                        : unit === "hours"
                          ? " hora(s)"
                          : unit === "minutes"
                            ? " minuto(s)"
                            : " segundo(s)"}
            </p>
        {/if}
    {/each}

    <!-- Declaração de amor -->
    <p class="mt-4 text-lg text-pink-700 italic">
        Que cada dia ao seu lado seja mais um capítulo lindo da nossa história.
        Te amo infinitamente!
    </p>

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
        will-change: transform;
        border: 1px solid black;
    }
</style>
