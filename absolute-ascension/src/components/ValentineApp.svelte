<script>
    export let couple;
    export let relationshipStartDate;
    export let images = [];

    import { onMount } from "svelte";
    import { writable } from "svelte/store";
    import { fade } from "svelte/transition";

    // Carrossel
    let currentImage = 0;
    const nextImage = () => (currentImage = (currentImage + 1) % images.length);
    const prevImage = () =>
        (currentImage = (currentImage - 1 + images.length) % images.length);

    import { onDestroy } from "svelte";

    let carouselInterval;

    onMount(() => {
        carouselInterval = setInterval(() => {
            nextImage();
        }, 3000); // Troca de imagem a cada 3 segundos

        return () => {
            clearInterval(carouselInterval);
        };
    });

    onDestroy(() => {
        clearInterval(carouselInterval);
    });

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

    let svgHearts = [];

    onMount(() => {
        const createSvgHeart = () => {
            const heart = {
                id: crypto.randomUUID?.() || Math.random(),
                left: Math.random() * 100, // % horizontal
                size: 20 + Math.random() * 20, // entre 20px e 40px
                duration: 4 + Math.random() * 3, // 4 a 7 segundos
                opacity: 0.4 + Math.random() * 0.4, // 0.4 a 0.8
                color: Math.random() > 0.5 ? "#e11d48" : "#db2777", // vermelho ou rosa
            };

            svgHearts = [...svgHearts, heart];

            // Remover após animação
            setTimeout(() => {
                svgHearts = svgHearts.filter((h) => h.id !== heart.id);
            }, heart.duration * 1000);
        };

        const interval = setInterval(createSvgHeart, 350);
        return () => clearInterval(interval);
    });

    const heartStyle = (heart) =>
        `left: ${heart.left}vw; width: ${heart.size}px; height: ${heart.size}px; opacity: ${heart.opacity}; animation: fall ${heart.duration}s linear forwards;`;

    let audioElement;
    let isPlaying = false;
    let volume = 0.5;

    let musicUrl = "https://youtu.be/xuiaL9HD2hw?si=xbK3AWK-wbNz3M4w"; // Substitua pelo link da música
    let musicTitle = "Nossa Canção Especial"; // Nome da música

    const togglePlay = () => {
        if (!audioElement) return;

        if (isPlaying) {
            audioElement.pause();
        } else {
            audioElement.play();
        }
        isPlaying = !isPlaying;
    };

    onMount(() => {
        if (audioElement) {
            audioElement.volume = volume;
        }
    });
</script>

<!-- Contêiner principal -->
<div class="relative w-full min-h-screen bg-pink-100">
    <div class="hearts-container">
        {#each svgHearts as heart (heart.id)}
            <svg
                class="absolute falling-heart"
                style={heartStyle(heart)}
                viewBox="0 0 24 24"
                fill={heart.color}
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5
                   2 5.42 4.42 3 7.5 3c1.74 0 3.41 0.81 4.5 2.09
                   C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42
                   22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
                />
            </svg>
        {/each}
    </div>
    <!-- Conteúdo principal -->
    <div class="relative z-10 w-full max-w-2xl mx-auto text-center py-8">
        <!-- Nomes do Casal -->
        <h1 class="text-4xl font-bold text-pink-600 mb-4">
            {couple.name1} ❤️ {couple.name2}
        </h1>

        <!-- Carrossel -->
        {#if images.length}
            <div
                class="relative mt-6 overflow-hidden rounded-lg shadow-lg h-64"
            >
                {#each images as image, index (index)}
                    {#if index === currentImage}
                        <img
                            src={image}
                            alt="Foto do casal"
                            class="absolute top-0 left-0 w-full h-full object-cover transition-opacity duration-700"
                            transition:fade
                        />
                    {/if}
                {/each}

                <button
                    on:click={prevImage}
                    class="absolute left-2 top-1/2 transform -translate-y-1/2 bg-pink-500 text-white p-2 rounded-full z-10"
                >
                    ←
                </button>
                <button
                    on:click={nextImage}
                    class="absolute right-2 top-1/2 transform -translate-y-1/2 bg-pink-500 text-white p-2 rounded-full z-10"
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
            Que cada dia ao seu lado seja mais um capítulo lindo da nossa
            história. Te amo infinitamente!
        </p>

        <!-- Player de Música -->
        <div
            class="mt-6 bg-white p-4 rounded-lg shadow-md flex items-center justify-between space-x-4"
        >
            <audio bind:this={audioElement} src={musicUrl}></audio>

            <button
                class="text-pink-600 hover:text-pink-800 text-2xl"
                on:click={togglePlay}
            >
                {isPlaying ? "⏸️" : "▶️"}
            </button>

            <div class="flex-1 text-left text-pink-800">
                <p class="font-semibold">Música do casal</p>
                <p class="text-sm italic">{musicTitle}</p>
            </div>

            <input
                type="range"
                min="0"
                max="1"
                step="0.01"
                bind:value={volume}
                on:input={() => {
                    if (audioElement) audioElement.volume = volume;
                }}
                class="w-24"
            />
        </div>
    </div>
</div>

<style>
    .hearts-container {
        position: fixed; /* fixa na viewport */
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        pointer-events: none; /* permite clicar no conteúdo abaixo */
        z-index: 0; /* atrás do conteúdo principal (que tem z-10) */
        border: 1px solid limegreen; /* borda verde para visualização */
    }

    .falling-heart {
        position: absolute;
        top: 0;
        will-change: transform, opacity;
    }

    @keyframes fall {
        0% {
            transform: translateY(-10vh);
            opacity: 1;
        }
        100% {
            transform: translateY(100vh);
            opacity: 0;
        }
    }
</style>
