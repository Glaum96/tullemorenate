<script>
  import { onMount, onDestroy } from 'svelte';
  import confetti from 'canvas-confetti';

  let { onDone } = $props();

  // Phases: 'countdown' | 'reveal' | 'done'
  let phase = $state('countdown');
  let countdownNum = $state(3);
  let countVisible = $state(true);

  // Reveal animation steps
  let showLine1 = $state(false);
  let showName = $state(false);
  let showLine2 = $state(false);
  let showScrollHint = $state(false);

  // Pre-compute particle positions to avoid re-calculation on re-render
  const particles = Array.from({ length: 20 }, () => ({
    x: Math.random() * 100,
    delay: Math.random() * 6,
    dur: 4 + Math.random() * 4,
  }));

  function fireConfetti() {
    const colors = ['#e8c97a', '#c9a84c', '#ffffff', '#f5e6c8', '#fffacd'];

    confetti({
      particleCount: 120,
      spread: 80,
      origin: { y: 0.4 },
      colors,
      scalar: 1.2,
    });

    setTimeout(() => {
      confetti({
        particleCount: 80,
        spread: 100,
        origin: { x: 0.1, y: 0.5 },
        colors,
        angle: 60,
      });
      confetti({
        particleCount: 80,
        spread: 100,
        origin: { x: 0.9, y: 0.5 },
        colors,
        angle: 120,
      });
    }, 300);
  }

  async function wait(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }

  function handleKey(e) {
    if (['ArrowDown', 'Enter', ' '].includes(e.key)) {
      e.preventDefault();
      onDone();
    }
  }

  function handleWheel(e) {
    if (e.deltaY > 0) onDone();
  }

  onMount(async () => {
    // Countdown: 3 → 2 → 1
    for (let n = 3; n >= 1; n--) {
      countdownNum = n;
      countVisible = true;
      await wait(800);
      countVisible = false;
      await wait(200);
    }

    // Switch to reveal phase
    phase = 'reveal';
    fireConfetti();

    await wait(400);
    showLine1 = true;
    await wait(700);
    showName = true;

    // Second confetti burst when name appears
    setTimeout(() => {
      confetti({ particleCount: 60, spread: 60, origin: { y: 0.35 }, colors: ['#e8c97a', '#c9a84c', '#ffffff'] });
    }, 300);

    await wait(900);
    showLine2 = true;
    await wait(1500);
    showScrollHint = true;

    window.addEventListener('keydown', handleKey);
    window.addEventListener('wheel', handleWheel, { passive: true });
  });

  onDestroy(() => {
    window.removeEventListener('keydown', handleKey);
    window.removeEventListener('wheel', handleWheel);
  });
</script>

<section class="intro" class:reveal={phase === 'reveal'}>
  <!-- Background particles -->
  <div class="particles">
    {#each particles as particle}
      <div class="particle" style="--x:{particle.x}%; --delay:{particle.delay}s; --dur:{particle.dur}s;"></div>
    {/each}
  </div>

  {#if phase === 'countdown'}
    <div class="countdown" class:visible={countVisible}>
      <span class="count-num">{countdownNum}</span>
    </div>
  {/if}

  {#if phase === 'reveal' || phase === 'done'}
    <div class="reveal-content">
      <div class="line1" class:visible={showLine1}>
        Gratulerer med 30-årsdagen
      </div>
      <div class="name-block" class:visible={showName}>
        <h1>Renate</h1>
        <div class="gold-line"></div>
      </div>
      <div class="line2" class:visible={showLine2}>
        <p>
          Du er omgitt av mennesker som er så utrolig glade i deg.<br />
          Scroll ned og se hvem som vil feire deg i dag. ♡
        </p>
      </div>
    </div>
  {/if}

  {#if showScrollHint}
    <button class="scroll-hint" onclick={onDone} type="button">
      <span>Scroll ned</span>
      <div class="arrow">↓</div>
    </button>
  {/if}
</section>

<style>
  .intro {
    position: relative;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background: radial-gradient(ellipse at center, #1a1a35 0%, #0d0d1a 70%);
    overflow: hidden;
    text-align: center;
    padding: 2rem;
  }

  /* Floating particles */
  .particles {
    position: absolute;
    inset: 0;
    pointer-events: none;
  }

  .particle {
    position: absolute;
    bottom: -10px;
    left: var(--x);
    width: 3px;
    height: 3px;
    border-radius: 50%;
    background: var(--gold-light);
    opacity: 0;
    animation: rise var(--dur) var(--delay) infinite ease-in;
  }

  @keyframes rise {
    0%   { opacity: 0; transform: translateY(0) scale(1); }
    10%  { opacity: 0.8; }
    80%  { opacity: 0.4; }
    100% { opacity: 0; transform: translateY(-100vh) scale(0.3); }
  }

  /* Countdown */
  .countdown {
    opacity: 0;
    transform: scale(2);
    transition: opacity 0.3s ease, transform 0.3s ease;
  }

  .countdown.visible {
    opacity: 1;
    transform: scale(1);
  }

  .count-num {
    font-family: var(--font-heading);
    font-size: clamp(8rem, 25vw, 15rem);
    font-weight: 900;
    color: var(--gold-light);
    text-shadow:
      0 0 40px rgba(232, 201, 122, 0.6),
      0 0 80px rgba(232, 201, 122, 0.3);
    line-height: 1;
  }

  /* Reveal content */
  .reveal-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1.5rem;
    max-width: 700px;
    width: 100%;
  }

  .line1 {
    font-family: var(--font-body);
    font-size: clamp(1rem, 3vw, 1.4rem);
    color: var(--text-muted);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.8s ease, transform 0.8s ease;
  }

  .line1.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .name-block {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 1s ease, transform 1s ease;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.8rem;
  }

  .name-block.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .name-block h1 {
    font-size: clamp(4rem, 14vw, 9rem);
    font-weight: 900;
    font-style: italic;
    background: linear-gradient(135deg, #f5e6c8, var(--gold-light), var(--gold));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    text-shadow: none;
    line-height: 1;
  }

  .gold-line {
    width: 120px;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--gold-light), transparent);
  }

  .line2 {
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 1s ease, transform 1s ease;
    max-width: 520px;
  }

  .line2.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .line2 p {
    font-size: clamp(1rem, 2.5vw, 1.2rem);
    font-style: italic;
    color: var(--text-primary);
    line-height: 1.8;
  }

  /* Scroll hint */
  .scroll-hint {
    position: absolute;
    bottom: 2.5rem;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.4rem;
    color: var(--text-muted);
    font-size: 0.9rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    cursor: pointer;
    animation: fadein 1s ease forwards;
    opacity: 0;
    transition: color 0.3s ease;
    background: none;
    border: none;
    font-family: var(--font-body);
  }

  .scroll-hint:hover {
    color: var(--gold-light);
  }

  @keyframes fadein {
    to { opacity: 1; }
  }

  .arrow {
    font-size: 1.4rem;
    animation: bounce 1.5s ease infinite;
  }

  @keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50%       { transform: translateY(6px); }
  }
</style>
