<div class="container">
  <div class="content">
    <h1>Noe er på vei</h1>
    <p>Følg med</p>
    <div class="dots">
      <span></span>
      <span></span>
      <span></span>
    </div>
  </div>
</div>

<style>
  .container {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    width: 100%;
  }

  .content {
    text-align: center;
  }

  h1 {
    font-family: var(--font-heading);
    font-size: clamp(2.5rem, 8vw, 5rem);
    font-weight: 700;
    color: var(--text-primary);
    margin-bottom: 1.5rem;
    letter-spacing: 0.05em;
  }

  p {
    font-family: var(--font-body);
    font-size: clamp(1rem, 3vw, 1.75rem);
    color: var(--gold-light);
    margin-bottom: 3rem;
    font-weight: 500;
    letter-spacing: 0.1em;
  }

  .dots {
    display: flex;
    gap: 0.75rem;
    justify-content: center;
  }

  .dots span {
    width: 0.5rem;
    height: 0.5rem;
    background-color: var(--gold);
    border-radius: 50%;
    animation: pulse 1.5s ease-in-out infinite;
  }

  .dots span:nth-child(2) {
    animation-delay: 0.2s;
  }

  .dots span:nth-child(3) {
    animation-delay: 0.4s;
  }

  @keyframes pulse {
    0%, 100% {
      opacity: 0.3;
      transform: scale(1);
    }
    50% {
      opacity: 1;
      transform: scale(1.2);
    }
  }

  @media (max-width: 640px) {
    h1 {
      margin-bottom: 1rem;
    }

    p {
      margin-bottom: 2rem;
    }
  }
</style>
