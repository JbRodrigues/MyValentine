<script>
    import { onMount } from "svelte";
    import { writable } from "svelte/store";
    export let relationshipStartDate;

    const startDate = new Date(relationshipStartDate);
    const timeTogether = writable({});

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

    onMount(() => {
        timeTogether.set(calculateTimeTogether());
        const interval = setInterval(() => {
            timeTogether.set(calculateTimeTogether());
        }, 1000);

        return () => clearInterval(interval);
    });
</script>

<h2 class="text-2xl font-bold text-pink-600 mb-4">
    Eu te amo a exatamente
</h2>

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
