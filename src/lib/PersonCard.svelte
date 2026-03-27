<script>
  import { onMount } from 'svelte';

  let {
    name,
    role = '',
    imageSrc = null,
    quote,
    videoSrc = null,
    align = 'left',   // 'left' | 'right' — which side the image is on desktop
  } = $props();

  let cardEl;
  let visible = $state(false);

  // Initials fallback
  const initials = name
    .split(' ')
    .map(w => w[0])
    .slice(0, 2)
    .join('')
    .toUpperCase();

  onMount(() => {
    const observer = new IntersectionObserver(
      ([entry]) => { if (entry.isIntersecting) { visible = true; observer.disconnect(); } },
      { threshold: 0.15 }
    );
    observer.observe(cardEl);
    return () => observer.disconnect();
  });
</script>

<article
  bind:this={cardEl}
  class="person-card"
  class:visible
  class:align-right={align === 'right'}
>
  <div class="avatar-col">
    <div class="avatar-ring">
      {#if imageSrc}
        <img src={imageSrc} alt={name} class="avatar-img" />
      {:else}
        <div class="avatar-placeholder">{initials}</div>
      {/if}
    </div>
    <div class="person-info">
      <div class="person-name">{name}</div>
      {#if role}
        <div class="person-role">{role}</div>
      {/if}
    </div>
  </div>

  <div class="quote-col">
    <div class="quote-mark">"</div>
    <blockquote class="quote-text">{quote}</blockquote>
    {#if videoSrc}
      <video class="card-video" src={videoSrc} controls playsinline>
        <track kind="captions" />
      </video>
    {/if}
  </div>
</article>

<style>
  .person-card {
    display: flex;
    flex-direction: column;
    gap: 2rem;
    padding: 2.5rem;
    background: var(--bg-card);
    border: 1px solid rgba(201, 168, 76, 0.15);
    border-radius: 1.5rem;
    max-width: 860px;
    margin: 0 auto;
    width: 100%;
    opacity: 0;
    transform: translateY(40px);
    transition: opacity 0.8s ease, transform 0.8s ease;
    box-shadow: 0 8px 40px rgba(0,0,0,0.4);
  }

  .person-card.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* Desktop: side-by-side layout */
  @media (min-width: 640px) {
    .person-card {
      flex-direction: row;
      align-items: flex-start;
      gap: 3rem;
    }

    .person-card.align-right {
      flex-direction: row-reverse;
    }
  }

  /* Avatar */
  .avatar-col {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
    flex-shrink: 0;
  }

  .avatar-ring {
    width: 130px;
    height: 130px;
    border-radius: 50%;
    padding: 3px;
    background: linear-gradient(135deg, var(--gold-light), var(--gold));
    flex-shrink: 0;
  }

  .avatar-img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    object-fit: cover;
  }

  .avatar-placeholder {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: var(--bg-secondary);
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: var(--font-heading);
    font-size: 2.2rem;
    font-weight: 700;
    color: var(--gold-light);
  }

  .person-info {
    text-align: center;
  }

  .person-name {
    font-family: var(--font-heading);
    font-size: 1.2rem;
    font-weight: 700;
    color: var(--gold-light);
  }

  .person-role {
    font-size: 0.9rem;
    color: var(--text-muted);
    font-style: italic;
    margin-top: 0.2rem;
  }

  /* Quote */
  .quote-col {
    flex: 1;
    position: relative;
    padding-top: 0.5rem;
  }

  .quote-mark {
    font-family: var(--font-heading);
    font-size: 5rem;
    line-height: 0.5;
    color: var(--gold-dark);
    opacity: 0.6;
    margin-bottom: 0.5rem;
    user-select: none;
  }

  .quote-text {
    font-family: var(--font-body);
    font-size: clamp(1rem, 2.5vw, 1.2rem);
    font-style: italic;
    color: var(--text-primary);
    line-height: 1.8;
    border: none;
    padding: 0;
  }

  .card-video {
    margin-top: 1.5rem;
    width: 100%;
    border-radius: 0.75rem;
    border: 1px solid rgba(201, 168, 76, 0.2);
  }
</style>
