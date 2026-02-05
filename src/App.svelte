<script>
  import { onDestroy } from 'svelte';

  let showCelebration = false;
  let noButtonRef;
  let fireworksInterval;
  let hasEscaped = false;

  function moveNoButton() {
    if (!noButtonRef) return;

    // On first escape, capture current position and switch to fixed
    if (!hasEscaped) {
      const rect = noButtonRef.getBoundingClientRect();
      noButtonRef.style.position = 'fixed';
      noButtonRef.style.left = rect.left + 'px';
      noButtonRef.style.top = rect.top + 'px';
      hasEscaped = true;

      // Force reflow, then animate to new position
      noButtonRef.offsetHeight;
    }

    const maxX = window.innerWidth - 150;
    const maxY = window.innerHeight - 80;
    const newX = Math.random() * maxX;
    const newY = Math.random() * maxY;

    noButtonRef.style.left = newX + 'px';
    noButtonRef.style.top = newY + 'px';
  }

  function celebrate() {
    showCelebration = true;
    createFireworks();

    // Add listeners to return to start
    setTimeout(() => {
      window.addEventListener('click', returnToStart);
      window.addEventListener('keydown', returnToStart);
    }, 100);
  }

  function returnToStart() {
    showCelebration = false;
    hasEscaped = false;
    window.removeEventListener('click', returnToStart);
    window.removeEventListener('keydown', returnToStart);
    if (fireworksInterval) {
      clearInterval(fireworksInterval);
      fireworksInterval = null;
    }
  }

  onDestroy(() => {
    window.removeEventListener('click', returnToStart);
    window.removeEventListener('keydown', returnToStart);
    if (fireworksInterval) clearInterval(fireworksInterval);
  });

  function createFireworks() {
    const container = document.querySelector('.fireworks-container');
    if (!container) return;

    for (let i = 0; i < 50; i++) {
      setTimeout(() => {
        const firework = document.createElement('div');
        firework.className = 'firework';
        firework.style.left = Math.random() * 100 + '%';
        firework.style.top = Math.random() * 100 + '%';
        firework.style.backgroundColor = `hsl(${Math.random() * 360}, 100%, 50%)`;
        container.appendChild(firework);

        setTimeout(() => firework.remove(), 1500);
      }, i * 100);
    }

    // Continue fireworks
    fireworksInterval = setInterval(() => {
      for (let i = 0; i < 10; i++) {
        setTimeout(() => {
          const firework = document.createElement('div');
          firework.className = 'firework';
          firework.style.left = Math.random() * 100 + '%';
          firework.style.top = Math.random() * 100 + '%';
          firework.style.backgroundColor = `hsl(${Math.random() * 360}, 100%, 50%)`;
          container.appendChild(firework);

          setTimeout(() => firework.remove(), 1500);
        }, i * 50);
      }
    }, 800);
  }
</script>

<main>
  {#if !showCelebration}
    <div class="hearts-bg">
      {#each Array(20) as _, i}
        <div class="floating-heart" style="left: {Math.random() * 100}%; animation-delay: {Math.random() * 5}s; font-size: {Math.random() * 20 + 10}px;">
          ❤️
        </div>
      {/each}
    </div>

    <div class="content">
      <div class="photo-frame">
        <img src="us.jpeg" alt="Us" class="couple-photo" />
      </div>

      <h1 class="question">Vil du være min vællentain, min kjæreste Renate?</h1>

      <div class="buttons">
        <button class="yes-btn" on:click={celebrate}>
          Yes
        </button>

        <button
          class="no-btn"
          bind:this={noButtonRef}
          on:mouseenter={moveNoButton}
          on:mousedown={moveNoButton}
        >
          No
        </button>
      </div>
    </div>
  {:else}
    <div class="celebration">
      <div class="fireworks-container"></div>
      <h1 class="celebration-text">YAAAAY!</h1>
      <p class="love-text">Jeg elsker deg, Renate! ❤️</p>
      <div class="hearts-explosion">
        {#each Array(30) as _, i}
          <span class="explosion-heart" style="--delay: {i * 0.1}s; --angle: {i * 12}deg;">❤️</span>
        {/each}
      </div>
    </div>
  {/if}
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    position: relative;
  }

  .hearts-bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    overflow: hidden;
  }

  .floating-heart {
    position: absolute;
    bottom: -50px;
    animation: float-up 8s linear infinite;
    opacity: 0.6;
  }

  @keyframes float-up {
    0% {
      transform: translateY(0) rotate(0deg);
      opacity: 0.6;
    }
    100% {
      transform: translateY(-110vh) rotate(360deg);
      opacity: 0;
    }
  }

  .content {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    padding: 2rem;
    box-sizing: border-box;
  }

  .photo-frame {
    width: 250px;
    height: 250px;
    border-radius: 50%;
    overflow: hidden;
    border: 8px solid #ff6b9d;
    box-shadow: 0 0 30px rgba(255, 107, 157, 0.5);
    margin-bottom: 2rem;
    background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 100%);
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .couple-photo {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .question {
    font-size: 2.5rem;
    color: #ff4d6d;
    text-align: center;
    margin-bottom: 2rem;
    text-shadow: 2px 2px 4px rgba(255, 77, 109, 0.3);
    max-width: 600px;
  }

  .buttons {
    display: flex;
    gap: 2rem;
    align-items: center;
  }

  .yes-btn {
    background: linear-gradient(135deg, #ff6b9d 0%, #ff4d6d 100%);
    color: white;
    border: none;
    padding: 1rem 3rem;
    font-size: 1.5rem;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 0 4px 15px rgba(255, 77, 109, 0.4);
  }

  .yes-btn:hover {
    transform: scale(1.1);
    box-shadow: 0 6px 25px rgba(255, 77, 109, 0.6);
  }

  .no-btn {
    background: #888;
    color: white;
    border: none;
    padding: 1rem 3rem;
    font-size: 1.5rem;
    border-radius: 50px;
    cursor: pointer;
    transition: left 0.3s ease-out, top 0.3s ease-out;
    z-index: 1000;
  }

  /* Celebration styles */
  .celebration {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 50%, #ff6b9d 100%);
    position: relative;
    overflow: hidden;
  }

  .fireworks-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 1;
  }

  :global(.firework) {
    position: absolute;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    animation: explode 1.5s ease-out forwards;
  }

  @keyframes explode {
    0% {
      transform: scale(0);
      opacity: 1;
    }
    50% {
      transform: scale(3);
      opacity: 1;
    }
    100% {
      transform: scale(0);
      opacity: 0;
    }
  }

  .celebration-text {
    font-size: 6rem;
    color: white;
    text-shadow: 4px 4px 8px rgba(255, 77, 109, 0.8);
    z-index: 10;
    animation: pulse 0.5s ease-in-out infinite alternate;
    margin: 0;
  }

  .love-text {
    font-size: 2.5rem;
    color: white;
    text-shadow: 2px 2px 4px rgba(255, 77, 109, 0.6);
    z-index: 10;
    margin-top: 1rem;
  }

  @keyframes pulse {
    from {
      transform: scale(1);
    }
    to {
      transform: scale(1.1);
    }
  }

  .hearts-explosion {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .explosion-heart {
    position: absolute;
    font-size: 2rem;
    animation: burst 2s ease-out infinite;
    animation-delay: var(--delay);
  }

  @keyframes burst {
    0% {
      transform: rotate(var(--angle)) translateY(0) scale(0);
      opacity: 1;
    }
    100% {
      transform: rotate(var(--angle)) translateY(-300px) scale(1.5);
      opacity: 0;
    }
  }
</style>
