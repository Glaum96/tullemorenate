<script>
  import { onMount } from 'svelte';

  let { part = 1, totalParts = 3 } = $props();

  // Auto-glob all images and videos from src/assets/gallery/
  const imageModules = import.meta.glob(
    '../assets/gallery/*.{jpg,jpeg,png,gif,webp,JPG,JPEG,PNG,GIF,WEBP}',
    { eager: true }
  );
  const videoModules = import.meta.glob(
    '../assets/gallery/*.{mp4,mov,webm,MP4,MOV,WEBM}',
    { eager: true }
  );

  function shuffle(arr) {
    const a = [...arr];
    for (let i = a.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [a[i], a[j]] = [a[j], a[i]];
    }
    return a;
  }

  // Combine all media and sort stably by path
  const allMedia = [
    ...Object.entries(imageModules).map(([path, m]) => ({ src: m.default, type: 'image', caption: '', path })),
    ...Object.entries(videoModules).map(([path, m]) => ({ src: m.default, type: 'video', caption: '', path })),
  ].sort((a, b) => a.path.localeCompare(b.path));

  // Split into parts
  const chunkSize = Math.ceil(allMedia.length / totalParts);
  const startIdx = (part - 1) * chunkSize;
  const endIdx = part * chunkSize;
  const partition = allMedia.slice(startIdx, endIdx);

  // Shuffle only this partition
  const images = shuffle(partition);

  // Preset rotations — cycles through so adjacent photos vary
  const rotations = [-3, 1.5, -1.5, 2.5, -2, 1, -4, 3, -0.5, 2];

  let sectionEl;
  let visible = $state(false);

  onMount(async () => {
    const observer = new IntersectionObserver(
      ([entry]) => { if (entry.isIntersecting) { visible = true; observer.disconnect(); } },
      { threshold: 0.1 }
    );
    observer.observe(sectionEl);
    return () => observer.disconnect();
  });

  // Headings per part
  const headings = {
    1: { label: 'Øyeblikk', title: 'Et lite utsnitt av livet ditt', text: 'Noen bilder som minner oss på hvem du er — og alt du fyller livet med.' },
    2: { label: 'Mer av deg', title: 'Enda flere minner', text: 'Øyeblikk som viser hvor fin du er — i alle slags lys.' },
    3: { label: 'Og litt til...', title: 'Fordi 95 bilder er akkurat passe', text: 'En siste samling av det som gjør deg til deg.' },
  };

  const heading = headings[part] || headings[1];
</script>

<section class="gallery-section" bind:this={sectionEl} class:visible>
  <div class="gallery-intro">
    <div class="gallery-label">{heading.label}</div>
    <h2 class="gallery-heading">{heading.title}</h2>
    <div class="gold-line"></div>
    <p class="gallery-subtext">
      {heading.text}
    </p>
  </div>

  <div class="photo-wall">
    {#if images.length > 0}
      {#each images as item, i}
        <div
          class="polaroid"
          style="--rot: {rotations[i % rotations.length]}deg; --delay: {i * 80}ms"
        >
          <div class="polaroid-img-wrap">
            {#if item.type === 'video'}
              <video src={item.src} playsinline muted loop autoplay>
                <track kind="captions" />
              </video>
            {:else}
              <img src={item.src} alt={item.caption} />
            {/if}
          </div>
          {#if item.caption}
            <p class="polaroid-caption">{item.caption}</p>
          {/if}
        </div>
      {/each}
    {:else}
      {#each Array(6) as _, i}
        <div
          class="polaroid"
          style="--rot: {rotations[i % rotations.length]}deg; --delay: {i * 80}ms"
        >
          <div class="polaroid-img-wrap placeholder-fill">
            <span>Bilde {i + 1}</span>
          </div>
        </div>
      {/each}
    {/if}
  </div>
</section>

<style>
  .gallery-section {
    padding: 5rem 1.5rem 6rem;
    background: linear-gradient(to bottom, var(--bg-primary), #111128, var(--bg-primary));
    overflow: hidden;
  }

  /* Intro */
  .gallery-intro {
    text-align: center;
    max-width: 580px;
    margin: 0 auto 4rem;
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.8s ease, transform 0.8s ease;
  }

  .gallery-section.visible .gallery-intro {
    opacity: 1;
    transform: translateY(0);
  }

  .gallery-label {
    font-family: var(--font-body);
    font-size: 0.9rem;
    letter-spacing: 0.4em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 0.6rem;
  }

  .gallery-heading {
    font-size: clamp(1.8rem, 5vw, 2.8rem);
    margin-bottom: 0.8rem;
  }

  .gold-line {
    width: 80px;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--gold-light), transparent);
    margin: 0 auto 1.2rem;
  }

  .gallery-subtext {
    font-style: italic;
    color: var(--text-muted);
    font-size: 1.05rem;
    line-height: 1.7;
  }

  /* Photo wall grid */
  .photo-wall {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
    gap: 2rem 1.5rem;
    max-width: 1000px;
    margin: 0 auto;
    padding: 2rem 0.5rem;
  }

  @media (min-width: 640px) {
    .photo-wall {
      grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
      gap: 2.5rem 2rem;
    }
  }

  /* Polaroid card */
  .polaroid {
    background: #f5efe6;
    padding: 0.7rem 0.7rem 2.2rem;
    box-shadow:
      0 4px 15px rgba(0,0,0,0.5),
      0 1px 3px rgba(0,0,0,0.3);
    transform: rotate(var(--rot));
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    opacity: 0;
    animation: none;
  }

  .gallery-section.visible .polaroid {
    animation: fadeup 0.6s ease var(--delay) forwards;
  }

  @keyframes fadeup {
    from { opacity: 0; transform: rotate(var(--rot)) translateY(25px); }
    to   { opacity: 1; transform: rotate(var(--rot)) translateY(0); }
  }

  .polaroid:hover {
    transform: rotate(0deg) scale(1.04) translateY(-4px);
    box-shadow:
      0 12px 35px rgba(0,0,0,0.6),
      0 4px 8px rgba(0,0,0,0.3);
    z-index: 10;
    position: relative;
  }

  .polaroid-img-wrap {
    width: 100%;
    aspect-ratio: 1 / 1;
    overflow: hidden;
    background: #ccc;
  }

  .polaroid-img-wrap img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }

  .placeholder-fill {
    background: linear-gradient(135deg, #2a2050, #1a1a35);
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .placeholder-fill span {
    color: rgba(255,255,255,0.2);
    font-style: italic;
    font-size: 0.9rem;
    font-family: var(--font-body);
  }

  .polaroid-caption {
    margin-top: 0.6rem;
    text-align: center;
    font-family: 'Caveat', var(--font-body), cursive;
    font-size: 0.85rem;
    color: #4a3f35;
    line-height: 1.3;
    min-height: 1.1em;
  }
</style>
