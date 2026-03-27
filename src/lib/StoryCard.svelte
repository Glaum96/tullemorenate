<script>
  import { onMount } from 'svelte';

  let {
    title,
    author,
    role = '',
    imageSrc = null,
    imageCaption = '',
    body,          // Full story text. Use \n\n to separate paragraphs.
    pullQuote = '', // Optional highlighted excerpt shown mid-story
  } = $props();

  const paragraphs = body.split('\n\n').map(p => p.trim()).filter(Boolean);

  // Split paragraphs around the pull quote position (after 2nd paragraph)
  const pullQuoteAfter = 1;

  let cardEl;
  let visible = $state(false);

  onMount(() => {
    const observer = new IntersectionObserver(
      ([entry]) => { if (entry.isIntersecting) { visible = true; observer.disconnect(); } },
      { threshold: 0.08 }
    );
    observer.observe(cardEl);
    return () => observer.disconnect();
  });
</script>

<article bind:this={cardEl} class="story-card" class:visible>

  <!-- Header -->
  <header class="story-header">
    <div class="ornament">✦</div>
    <h3 class="story-title">{title}</h3>
    <div class="story-byline">
      <span class="story-author">{author}</span>
      {#if role}
        <span class="story-role">— {role}</span>
      {/if}
    </div>
    <div class="header-rule"></div>
  </header>

  <!-- Optional image -->
  {#if imageSrc}
    <figure class="story-image">
      <img src={imageSrc} alt={imageCaption || title} />
      {#if imageCaption}
        <figcaption>{imageCaption}</figcaption>
      {/if}
    </figure>
  {/if}

  <!-- Body text with optional pull quote woven in -->
  <div class="story-body">
    {#each paragraphs as paragraph, i}
      <p class:dropcap={i === 0}>{paragraph}</p>
      {#if pullQuote && i === pullQuoteAfter}
        <blockquote class="pull-quote">
          <span class="pq-mark">"</span>
          {pullQuote}
          <span class="pq-mark">"</span>
        </blockquote>
      {/if}
    {/each}
  </div>

  <!-- Footer ornament -->
  <footer class="story-footer">
    <div class="footer-rule"></div>
    <div class="footer-ornament">✦ ✦ ✦</div>
  </footer>

</article>

<style>
  .story-card {
    background: var(--bg-card);
    border: 1px solid rgba(201, 168, 76, 0.15);
    border-radius: 1.5rem;
    max-width: 760px;
    margin: 0 auto;
    width: 100%;
    padding: 3rem 2.5rem;
    opacity: 0;
    transform: translateY(40px);
    transition: opacity 0.9s ease, transform 0.9s ease;
    box-shadow: 0 8px 40px rgba(0, 0, 0, 0.4);
  }

  @media (min-width: 640px) {
    .story-card {
      padding: 4rem 4.5rem;
    }
  }

  .story-card.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* Header */
  .story-header {
    text-align: center;
    margin-bottom: 2.5rem;
  }

  .ornament {
    font-size: 1.2rem;
    color: var(--gold);
    opacity: 0.7;
    margin-bottom: 0.8rem;
    letter-spacing: 0.3em;
  }

  .story-title {
    font-size: clamp(1.5rem, 4vw, 2.2rem);
    font-style: italic;
    color: var(--gold-light);
    margin-bottom: 0.6rem;
  }

  .story-byline {
    font-size: 0.95rem;
    color: var(--text-muted);
    letter-spacing: 0.05em;
  }

  .story-author {
    font-family: var(--font-heading);
    font-weight: 600;
    color: var(--gold);
  }

  .story-role {
    font-style: italic;
  }

  .header-rule {
    width: 60px;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--gold-dark), transparent);
    margin: 1.4rem auto 0;
  }

  /* Image */
  .story-image {
    margin: 0 0 2.5rem;
    border-radius: 0.75rem;
    overflow: hidden;
    border: 1px solid rgba(201, 168, 76, 0.15);
  }

  .story-image img {
    width: 100%;
    max-height: 420px;
    object-fit: cover;
    display: block;
  }

  .story-image figcaption {
    text-align: center;
    padding: 0.7rem 1rem;
    font-style: italic;
    font-size: 0.88rem;
    color: var(--text-muted);
    background: rgba(0,0,0,0.2);
  }

  /* Body */
  .story-body {
    font-size: clamp(1.05rem, 2vw, 1.18rem);
    line-height: 1.95;
    color: var(--text-primary);
  }

  .story-body p {
    margin-bottom: 1.4em;
  }

  /* Drop cap on first paragraph */
  .story-body p.dropcap::first-letter {
    font-family: var(--font-heading);
    font-size: 4em;
    font-weight: 900;
    color: var(--gold-light);
    float: left;
    line-height: 0.75;
    margin: 0.1em 0.12em 0 0;
  }

  /* Pull quote */
  .pull-quote {
    margin: 2rem -1rem;
    padding: 1.5rem 2rem;
    border-left: 3px solid var(--gold);
    background: rgba(201, 168, 76, 0.06);
    border-radius: 0 0.5rem 0.5rem 0;
    font-family: var(--font-heading);
    font-size: clamp(1.1rem, 3vw, 1.45rem);
    font-style: italic;
    color: var(--gold-light);
    line-height: 1.6;
  }

  @media (min-width: 640px) {
    .pull-quote {
      margin: 2rem -2.5rem;
    }
  }

  .pq-mark {
    font-size: 1.6em;
    line-height: 0;
    vertical-align: -0.3em;
    color: var(--gold-dark);
    margin: 0 0.1em;
    font-style: normal;
  }

  /* Footer */
  .story-footer {
    text-align: center;
    margin-top: 2rem;
  }

  .footer-rule {
    width: 100%;
    height: 1px;
    background: linear-gradient(90deg, transparent, rgba(201,168,76,0.2), transparent);
    margin-bottom: 1.2rem;
  }

  .footer-ornament {
    font-size: 0.7rem;
    letter-spacing: 0.5em;
    color: var(--gold-dark);
    opacity: 0.6;
  }
</style>
