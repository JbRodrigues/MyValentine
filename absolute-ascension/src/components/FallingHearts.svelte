<script>
    import { onMount } from "svelte";
    import { writable } from "svelte/store";

    const svgHearts = writable([]);

    const createSvgHeart = () => {
        const heart = {
            id: crypto.randomUUID?.() || Math.random(),
            left: Math.random() * 100,
            size: 20 + Math.random() * 20,
            duration: 4 + Math.random() * 3,
            opacity: 0.4 + Math.random() * 0.4,
            color: Math.random() > 0.5 ? "#e11d48" : "#db2777",
        };

        svgHearts.update((h) => [...h, heart]);

        setTimeout(() => {
            svgHearts.update((h) => h.filter((x) => x.id !== heart.id));
        }, heart.duration * 1000);
    };

    onMount(() => {
        const interval = setInterval(createSvgHeart, 350);
        return () => clearInterval(interval);
    });

    const heartStyle = (heart) =>
        `left: ${heart.left}vw; width: ${heart.size}px; height: ${heart.size}px; opacity: ${heart.opacity}; animation: fall ${heart.duration}s linear forwards;`;
</script>

<div class="hearts-container">
    {#each $svgHearts as heart (heart.id)}
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

<style>
    .hearts-container {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        pointer-events: none;
        z-index: 0;
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
