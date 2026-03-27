<script>
  const CORRECT_PASSWORD = 'cleanboy30';

  let { onUnlock } = $props();

  let inputValue = $state('');
  let shaking = $state(false);
  let wrongAttempt = $state(false);

  // Pre-compute star positions to avoid re-calculation on re-render
  const stars = Array.from({ length: 30 }, () => ({
    x: Math.random() * 100,
    y: Math.random() * 100,
    delay: Math.random() * 5,
    size: Math.random() * 2 + 1,
  }));

  function handleSubmit(e) {
    e.preventDefault();
    if (inputValue === CORRECT_PASSWORD) {
      onUnlock();
    } else {
      wrongAttempt = true;
      shaking = true;
      setTimeout(() => { shaking = false; }, 600);
      inputValue = '';
    }
  }
</script>

<div class="password-page">
  <!-- Animated star particles -->
  <div class="stars">
    {#each stars as star}
      <div class="star" style="--x:{star.x}%; --y:{star.y}%; --delay:{star.delay}s; --size:{star.size}px;"></div>
    {/each}
  </div>

  <div class="content">
    <div class="sparkle-icon">✨</div>
    <h1>For verdens beste menneske</h1>
    <p class="subtitle">Skriv inn passordet for å komme inn</p>

    <form onsubmit={handleSubmit} class:shake={shaking}>
      <input
        type="password"
        bind:value={inputValue}
        placeholder="Passord"
        autocomplete="off"
        autofocus
      />
      {#if wrongAttempt}
        <p class="error-hint">Feil passord, prøv igjen</p>
      {/if}
      <button type="submit">Kom inn</button>
    </form>
  </div>
</div>

<style>
  .password-page {
    position: relative;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: radial-gradient(ellipse at center, #1a1a35 0%, #0d0d1a 70%);
    overflow: hidden;
  }

  /* Stars */
  .stars {
    position: absolute;
    inset: 0;
    pointer-events: none;
  }

  .star {
    position: absolute;
    left: var(--x);
    top: var(--y);
    width: var(--size);
    height: var(--size);
    border-radius: 50%;
    background: var(--gold-light);
    opacity: 0;
    animation: twinkle 4s var(--delay) infinite;
  }

  @keyframes twinkle {
    0%, 100% { opacity: 0; transform: scale(1); }
    50%       { opacity: 0.8; transform: scale(1.5); }
  }

  /* Content */
  .content {
    position: relative;
    z-index: 1;
    text-align: center;
    padding: 2rem;
    max-width: 420px;
    width: 100%;
  }

  .sparkle-icon {
    font-size: 3.5rem;
    margin-bottom: 1rem;
    animation: float 3s ease-in-out infinite;
  }

  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50%       { transform: translateY(-10px); }
  }

  h1 {
    font-size: clamp(1.8rem, 5vw, 2.8rem);
    margin-bottom: 0.5rem;
  }

  .subtitle {
    color: var(--text-muted);
    font-size: 1.1rem;
    margin-bottom: 2.5rem;
    font-style: italic;
  }

  /* Form */
  form {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    align-items: center;
  }

  form.shake {
    animation: shake 0.6s ease;
  }

  @keyframes shake {
    0%, 100% { transform: translateX(0); }
    15%       { transform: translateX(-8px); }
    30%       { transform: translateX(8px); }
    45%       { transform: translateX(-6px); }
    60%       { transform: translateX(6px); }
    75%       { transform: translateX(-4px); }
    90%       { transform: translateX(4px); }
  }

  input {
    width: 100%;
    max-width: 320px;
    padding: 1rem 1.5rem;
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid var(--gold-dark);
    border-radius: 3rem;
    color: var(--text-primary);
    font-family: var(--font-body);
    font-size: 1.1rem;
    text-align: center;
    letter-spacing: 0.2em;
    transition: var(--transition);
  }

  input::placeholder {
    color: var(--text-muted);
    letter-spacing: 0.05em;
  }

  input:focus {
    border-color: var(--gold-light);
    background: rgba(255, 255, 255, 0.08);
    box-shadow: 0 0 20px rgba(201, 168, 76, 0.2);
  }

  .error-hint {
    color: var(--error);
    font-size: 0.9rem;
    font-style: italic;
    margin-top: -0.5rem;
  }

  button {
    padding: 0.9rem 3rem;
    background: linear-gradient(135deg, var(--gold-light), var(--gold));
    color: var(--bg-primary);
    font-family: var(--font-heading);
    font-size: 1.1rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    border-radius: 3rem;
    transition: var(--transition);
    box-shadow: 0 4px 20px rgba(201, 168, 76, 0.3);
  }

  button:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 30px rgba(201, 168, 76, 0.5);
  }

  button:active {
    transform: translateY(0);
  }
</style>
