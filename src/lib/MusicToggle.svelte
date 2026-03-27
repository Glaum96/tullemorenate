<script>
  import { onMount, onDestroy } from 'svelte';

  // Replace with real audio file path when ready
  const AUDIO_SRC = '/music.mp3';

  let playing = $state(false);
  let audio;

  onMount(() => {
    audio = new Audio(AUDIO_SRC);
    audio.loop = true;
    audio.volume = 0.4;
  });

  onDestroy(() => {
    if (audio) {
      audio.pause();
      audio = null;
    }
  });

  function toggle() {
    if (!audio) return;
    if (playing) {
      audio.pause();
      playing = false;
    } else {
      audio.play().catch(() => {
        // Autoplay blocked — user must interact first, which they did
      });
      playing = true;
    }
  }
</script>

<button
  class="music-toggle"
  class:playing
  onclick={toggle}
  title={playing ? 'Skru av musikk' : 'Skru på musikk'}
  aria-label={playing ? 'Skru av musikk' : 'Skru på musikk'}
>
  {#if playing}
    <span class="icon">♫</span>
  {:else}
    <span class="icon muted">♩</span>
  {/if}
  {#if playing}
    <span class="ring ring1"></span>
    <span class="ring ring2"></span>
  {/if}
</button>

<style>
  .music-toggle {
    position: fixed;
    bottom: 2rem;
    right: 2rem;
    z-index: 1000;
    width: 52px;
    height: 52px;
    border-radius: 50%;
    background: rgba(13, 13, 26, 0.85);
    border: 1px solid var(--gold-dark);
    color: var(--gold-light);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.4rem;
    backdrop-filter: blur(8px);
    transition: border-color 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 4px 20px rgba(0,0,0,0.4);
  }

  .music-toggle:hover {
    border-color: var(--gold-light);
    box-shadow: 0 4px 25px rgba(201, 168, 76, 0.35);
  }

  .music-toggle.playing {
    border-color: var(--gold);
  }

  .icon {
    position: relative;
    z-index: 1;
    line-height: 1;
  }

  .icon.muted {
    opacity: 0.5;
  }

  /* Animated rings when playing */
  .ring {
    position: absolute;
    inset: -1px;
    border-radius: 50%;
    border: 1px solid var(--gold);
    opacity: 0;
    animation: ripple 2s ease-out infinite;
  }

  .ring2 {
    animation-delay: 1s;
  }

  @keyframes ripple {
    0%   { transform: scale(1); opacity: 0.6; }
    100% { transform: scale(1.8); opacity: 0; }
  }
</style>
