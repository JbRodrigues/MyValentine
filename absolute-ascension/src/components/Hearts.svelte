<script>
  import { onMount } from "svelte";

  let svgHearts = [];

  onMount(() => {
    const createSvgHeart = () => {
      const heart = {
        id: Math.random(),
        left: Math.random() * 100,
        size: 30 + Math.random() * 20,
        duration: 5 + Math.random() * 3,
        opacity: 0.5 + Math.random() * 0.5,
        color: Math.random() > 0.5 ? "#e11d48" : "#db2777",
      };
      svgHearts = [...svgHearts, heart];

      setTimeout(() => {
        svgHearts = svgHearts.filter((h) => h.id !== heart.id);
      }, heart.duration * 1000);
    };

    const interval = setInterval(createSvgHeart, 300);
    return () => clearInterval(interval);
  });

  const heartStyle = (heart) =>
    `left: ${heart.left}vw; width: ${heart.size}px; height: ${heart.size}px; opacity: ${heart.opacity}; animation: fall ${heart.duration}s linear forwards; position: absolute; top: 0;`;
</script>

<div class="hearts-container">
  {#each svgHearts as heart (heart.id)}
    <svg
      class="falling-heart"
      style={heartStyle(heart)}
      viewBox="0 0 24 24"
      fill={heart.color}
      xmlns="http://www.w3.org/2000/svg"
    >
      <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5
        2 5.42 4.42 3 7.5 3c1.74 0 3.41 0.81 4.5 2.09
        C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42
        22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z" />
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
    background: pink; /* para você ver a área */
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
