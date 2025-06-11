<script>
    export let musicUrl;
    export let musicTitle;
    let audioElement;
    let isPlaying = false;
    let volume = 0.5;

    const togglePlay = () => {
        if (!audioElement) return;
        if (isPlaying) {
            audioElement.pause();
        } else {
            audioElement.play();
        }
        isPlaying = !isPlaying;
    };

    const updateVolume = () => {
        if (audioElement) audioElement.volume = volume;
    };
</script>

<div class="mt-6 bg-white p-4 rounded-lg shadow-md flex items-center justify-between space-x-4">
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
        on:input={updateVolume}
        class="w-24"
    />
</div>
