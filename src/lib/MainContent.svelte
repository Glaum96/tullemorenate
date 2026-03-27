<script>
  import PersonCard from "./PersonCard.svelte";
  import PhotoGallery from "./PhotoGallery.svelte";
  import StoryCard from "./StoryCard.svelte";
  import { onMount } from "svelte";

  // Placeholder persons — replace with real data later
  const persons = [
    {
      name: "Bjørg",
      role: "Mor",
      imageSrc: "bjørg.png",
      quote:
        'Hun kan være sårbar, men viser sitt "jävlar anamma" når det behøves. Elsker at folk tar vare på henne, både som kjæreste, venn og familiemedlem. Og stiller selv opp for de som måtte behøve det, selv om de ikke alltid skjønner det selv. Er utrolig vittig og fandenivoldsk med kommentarer som man kan le seg ihjel av. Er god på å følge drømmene sine og er fremdeles åpen for nye eventyr, selv i den modne alder av 30 år.',
      videoSrc: null,
    },
    {
      name: "Anne",
      role: "Bestevenn",
      imageSrc: "anne.jpeg",
      quote: "Renate er en utrolig person. Smart, morsom, klok, kreativ, sinnsyk organisator, snill, god å planlegge og ikke minst vakker er noen av de ordene som kan beskrive henne. Hun er en trygg havn for alle sine venner og min bedre halvdel for alt det er verdt. De siste 16-ish årene har vært et lysere sted på grunn av hennes hjertelige monsterlatter, gode råd og omsorg. Glad i deg, Kakis❤️",
      videoSrc: null,
    },
    {
      name: "Sara",
      role: "Bestevenn",
      imageSrc: "sara.jpeg",
      quote:
        "Renate er et veldig godt menneske å være venn med! Ho vil alltid høre på stor og smått av drama og (u)viktige hendelser i ditt liv, og viser tydelig at ho er engasjert. Sjølv om eg var sammen med hennes eks (husker ikkje heilt ka han heiter… Toffee noe?) blei eg tatt VARMT imot, og eg følte nesten ho valgte meg som venninne der og da. For et privilegium! Renate kan masse rart og får til mye. Eg blir aldri overraska over alt mestrer (hallo FN??), men alltid imponert! Det er veldig godt å ha ho på sitt lag - både som ein strukturert og kapabel sparringspartner, og som ein sprudlende heiagjeng. Utvalgte ord som beskriver Renate: Varm, klok, artig, morsom, PEN, kul, jordnær, hips don’t lie, TikTok-proff, artikulert, kreativ, passenger princess, Anne’s støttekontakt, kåpe, «hater» på riktig måte, boobies.Eg unner Renate ny og enda mer spennende jobb, trygg kjærlighet og masse eventyr i 30-årene. Eg håper ho får alt ho drømmer om, og gleder meg til å ta del i dei mange oppturene og dei (sjukt få) nedturene! Glad i deg ❤️",
      videoSrc: null,
    },
    {
      name: "Silje",
      role: "Bestevenn",
      imageSrc: "silje.png",
      quote: "Renate er verdens aller beste bestevenn i hele verden❤️ Ingen blir så glad på andres vegne, er så morsom og gøy, varm og omtenksom, raus og vanvittig omsorgsfull, tidens beste og kuleste organisator, venn og menneske ❤️ Du snudde livet mitt opp ned fra øyeblikket du kom inn i det, og eg kan aldri tenke meg et liv uten! Klisjéen er sann: ord strekker ikke til når eg skal beskrive deg Renate ❤️ eg digger hele deg, og du hadde vært verdt alle årene mine i Bergen helt alene! Kan ikke tro hvor heldig eg er som har en bestevenn som deg i livet mitt ❤️ Ingen viser kjærlighet som du gjør til folka dine rundt deg! Du er min ride or die, partner in crime, andre halvdel av vår dynamic duo. Gleder meg til du skal være tante til mine barn, til vi skal feriere (enda mer) sammen og alle livets oppturer og nedturer vi skal dele sammen ❤️ eg vil alltid, alltid være her for deg, alltid støtte deg, alltid være din største heiagjeng og alltid love you to biiiiits❤️ takk for alle gangene du har spurt meg hvordan eg har det, lyttet til meg, støttet meg, grått med meg, ledd med meg, dansa med meg, sunget med meg og opplevd livets berg og dalbane sammen ❤️ eg sa til Haakon at han måtte gi meg en ordbegrensning ellers kom eg aldri til å slutte 😂 I LOVE YOU MY LOVE",
      videoSrc: "britney.mp4",
    },
    {
      name: "Haakon",
      role: "Samboer",
      imageSrc: "/us.jpeg",
      quote:
        "Gratulerer så sinnsykt mye med 30-årsdagen, mitt favorittmenneske! Jeg føler meg så takknemmlig for å være din og å tenke på at du er min. Du er det beste som har skjedd meg. Gleder meg til å være clean boy-ork med deg i mange tiår til. Elsker deg så mye ❤️",
      videoSrc: 'dnd.MP4',
    },
    {
      name: "Christina",
      role: "Sjef",
      imageSrc: "christina.jpg",
      quote:
        "Kjære Renate  Takk for at jeg får ha kule og fantastiske deg som både kollega og venn! Evig takknemlig for all støtte, råd, latter, frustrasjon og mer vi deler på jobb. Takk for at du gjør min første erfaring som personalleder så fin og god. Øyeblikkene, sitatene og opplevelsene begynner å bli så mange at det er vanskelig å huske på dem! Men både HU-kontoret og jeg savner bergensdialekt og bergensere når du er borte over lengre tid.  Du er rå, og ønsker deg alt godt for både dagen i dag og livet videre!",
      videoSrc: null,
    },
    {
      name: "Alida",
      role: "SV-sjef",
      imageSrc: "alida.jpg",
      quote:
        "Renate er pliktoppfyllende, glad og dyktig! Veldig bra nestledermaterialet✊ Litt mye Bergen innimellom, trenger ikke nevne Bergen hvert styremøte (sakt med mye kjærlighet❤️). Veldig takknemlig for å ha henne i styret og livet! Ønsker henne alt bra for fremtiden💃🏼",
      videoSrc: null,
    },
    {
      name: "Emma",
      role: "Venn",
      imageSrc: "emma.jpg",
      quote:
        "Howdy min aries fire partner! Takk for at vi kan samles om vold, og hete dance moves på stranda i Hellas  Du er alltid et fyrverkeri og sprer masse glede og latter🤟🏼🤟🏼!",
      videoSrc: 'slap.mp4',
    },
    {
      name: "Kristin",
      role: "Svigermor",
      imageSrc: "kristin.jpeg",
      quote: "Glad for å ha blitt kjent med deg, Renate!",
      videoSrc: null,
    },
    {
      name: "Susann",
      role: "Venn",
      imageSrc: "susann.jpeg",
      quote: "du er jævlig kul",
      videoSrc: null,
    },
    {
      name: "Emilie",
      role: "Venn",
      imageSrc: "emilie.jpeg",
      quote: "du er kul. du er morsom. du er pen. jeg savner det røde håret ditt. renataaa 🔥",
      videoSrc: null,
    },
    {
      name: "Øistein",
      role: "Venn",
      imageSrc: "øistein.jpeg",
      quote:
        "Det beste med Renate er hvor enkelt det er å overbevise henne om å dra på konserter hun egentlig er middels interresert i ❤️",
      videoSrc: null,
    },
  ];

  const stories = [
    {
      title: "Story Time with Bjørg",
      author: "Bjørg",
      role: "Mor",
      imageSrc: null,
      imageCaption: "",
      body: `Da Renate (og Ingrid) gikk i skolekorpset gadd ikke Renate øve særlig mye på kornetten. Så da de marsjerte og spilte, bare sang hun duuuuduuuuduuu i munnstykket på de partiene hun ikke klarte å spille.  
      Hun har alltid hatt en veldig rettferdighetssans og har stått i mange kamper, både for seg selv og hvilke som helst andre som ikke klarte det selv. Hun har kjeftet mye på lærere…
      Er utrolig morsom, både i tale og tekst! Hennes tale i bryllupet til Ingrid og Marius ler folk av ennå!
      Er så glad for at hun fulgte drømmen sin og flyttet til Oslo for å jobbe der, selv om det medfører at vi får treffe henne mer sjelden enn før.
      Har iblant hofter og knær som en gammel dame, men vi ligger alltid 50 meter bak henne når vi er ute og går med familiens Speedy Gonzales.`,
    },
  ];

  // Split persons: first half → gallery → second half
  const personsFirst = $derived(
    persons.slice(0, Math.ceil(persons.length / 2)),
  );
  const personsSecond = $derived(persons.slice(Math.ceil(persons.length / 2)));

  let heroEl;
  let scrollY = $state(0);

  onMount(() => {
    const onScroll = () => {
      scrollY = window.scrollY;
    };
    window.addEventListener("scroll", onScroll, { passive: true });
    return () => window.removeEventListener("scroll", onScroll);
  });
</script>

<main class="main-content">
  <!-- ===== HERO ===== -->
  <section class="hero" bind:this={heroEl}>
    <!-- Parallax background layer -->
    <div class="hero-bg" style="transform: translateY({scrollY * 0.3}px)">
      <!-- Replace with real Renate photo: <img src="/renate-hero.jpg" alt="Renate" /> -->
      <div class="hero-placeholder">
        <span class="hero-placeholder-text">Bilde av Renate</span>
      </div>
    </div>

    <div class="hero-overlay"></div>

    <div class="hero-text">
      <div class="hero-label">30 år</div>
      <h1 class="hero-name">Renate</h1>
      <div class="gold-divider"></div>
      <p class="hero-subtitle">Tre tiår med sol, latter og kjærlighet</p>
    </div>
  </section>

  <!-- ===== INTRO BLURB ===== -->
  <section class="blurb-section">
    <div class="blurb-inner">
      <p class="blurb-text">
        Her er samlet noen ord fra menneskene som er aller mest glade i deg. De
        som kjenner deg, og som vet akkurat hva som gjør deg til den fantastiske
        personen du er.
      </p>
    </div>
  </section>

  <!-- ===== PERSONS ===== -->
  <section class="persons-section">
    <h2 class="section-heading">Menneskene som elsker deg</h2>
    <div class="gold-divider centered"></div>

    <div class="cards-list">
      {#each personsFirst as person, i}
        <PersonCard
          name={person.name}
          role={person.role}
          imageSrc={person.imageSrc}
          quote={person.quote}
          videoSrc={person.videoSrc}
          align={i % 2 === 0 ? "left" : "right"}
        />
      {/each}
    </div>
  </section>

  <!-- ===== PHOTO GALLERY (Part 1) ===== -->
  <PhotoGallery part={1} totalParts={3} />

  <!-- ===== PERSONS (second half) ===== -->
  <section class="persons-section persons-section-continued">
    <div class="cards-list">
      {#each personsSecond as person, i}
        <PersonCard
          name={person.name}
          role={person.role}
          imageSrc={person.imageSrc}
          quote={person.quote}
          videoSrc={person.videoSrc}
          align={i % 2 === 0 ? "right" : "left"}
        />
      {/each}
    </div>
  </section>

  <!-- ===== PHOTO GALLERY (Part 2) ===== -->
  <PhotoGallery part={2} totalParts={3} />

  <!-- ===== STORIES ===== -->
  <section class="stories-section">
    <div class="gold-divider centered"></div>
    <div class="stories-list">
      {#each stories as story}
        <StoryCard
          title={story.title}
          author={story.author}
          role={story.role}
          imageSrc={story.imageSrc}
          imageCaption={story.imageCaption}
          body={story.body}
          pullQuote={story.pullQuote}
        />
      {/each}
    </div>
  </section>

  <!-- ===== PHOTO GALLERY (Part 3) ===== -->
  <PhotoGallery part={3} totalParts={3} />

  <!-- ===== CLOSING ===== -->
  <section class="closing-section">
    <div class="closing-inner">
      <div class="closing-heart">♡</div>
      <h2>Gratulerer med 30-årsdagen, Renate</h2>
      <div class="gold-divider centered"></div>
      <div class="closing-message">
        <p class="closing-label">[Skriv din kjærlighetserklæring her]</p>
        <p class="closing-text">
          — Haakon
        </p>
      </div>
    </div>
  </section>
</main>

<style>
  .main-content {
    background: var(--bg-primary);
  }

  /* ===== HERO ===== */
  .hero {
    position: relative;
    height: 100vh;
    min-height: 500px;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }

  .hero-bg {
    position: absolute;
    inset: -20%;
    will-change: transform;
  }

  .hero-bg :global(img) {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .hero-placeholder {
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, #1a1a35 0%, #2a2050 50%, #1a1a35 100%);
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .hero-placeholder-text {
    color: var(--text-muted);
    font-style: italic;
    font-size: 1rem;
    opacity: 0.4;
  }

  .hero-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(
      to bottom,
      rgba(13, 13, 26, 0.2) 0%,
      rgba(13, 13, 26, 0.3) 40%,
      rgba(13, 13, 26, 0.85) 100%
    );
  }

  .hero-text {
    position: relative;
    z-index: 1;
    text-align: center;
    padding: 2rem;
  }

  .hero-label {
    font-family: var(--font-body);
    font-size: clamp(1rem, 3vw, 1.3rem);
    letter-spacing: 0.4em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 0.5rem;
  }

  .hero-name {
    font-size: clamp(4rem, 14vw, 10rem);
    font-weight: 900;
    font-style: italic;
    background: linear-gradient(
      135deg,
      #f5e6c8,
      var(--gold-light),
      var(--gold)
    );
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1;
    margin-bottom: 1rem;
    text-shadow: none;
  }

  .hero-subtitle {
    font-style: italic;
    color: var(--text-muted);
    font-size: clamp(1rem, 2.5vw, 1.2rem);
    margin-top: 1rem;
  }

  /* ===== DIVIDER ===== */
  .gold-divider {
    width: 80px;
    height: 1px;
    background: linear-gradient(
      90deg,
      transparent,
      var(--gold-light),
      transparent
    );
    margin: 0.8rem 0;
  }

  .gold-divider.centered {
    margin: 0.8rem auto;
  }

  /* ===== BLURB ===== */
  .blurb-section {
    padding: 5rem 2rem;
    text-align: center;
  }

  .blurb-inner {
    max-width: 640px;
    margin: 0 auto;
  }

  .blurb-text {
    font-size: clamp(1.1rem, 2.5vw, 1.35rem);
    font-style: italic;
    color: var(--text-primary);
    line-height: 1.9;
  }

  /* ===== PERSONS ===== */
  .persons-section-continued {
    padding-top: 2rem;
  }

  .persons-section {
    padding: 4rem 1.5rem 6rem;
    max-width: 960px;
    margin: 0 auto;
  }

  .section-heading {
    text-align: center;
    font-size: clamp(1.8rem, 5vw, 3rem);
    margin-bottom: 0.5rem;
  }

  .cards-list {
    display: flex;
    flex-direction: column;
    gap: 3rem;
    margin-top: 4rem;
  }

  /* ===== STORIES ===== */
  .stories-section {
    padding: 4rem 1.5rem 6rem;
    max-width: 960px;
    margin: 0 auto;
  }

  .stories-list {
    display: flex;
    flex-direction: column;
    gap: 4rem;
    margin-top: 4rem;
  }

  /* ===== CLOSING ===== */
  .closing-section {
    padding: 6rem 2rem 8rem;
    text-align: center;
    background: linear-gradient(to bottom, var(--bg-primary), #1a1a35);
  }

  .closing-inner {
    max-width: 580px;
    margin: 0 auto;
  }

  .closing-heart {
    font-size: 3.5rem;
    color: var(--gold-light);
    margin-bottom: 1.5rem;
    display: block;
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%,
    100% {
      transform: scale(1);
      opacity: 1;
    }
    50% {
      transform: scale(1.1);
      opacity: 0.8;
    }
  }

  .closing-inner h2 {
    font-size: clamp(1.4rem, 4vw, 2.2rem);
    margin-bottom: 1rem;
  }

  .closing-message {
    margin-top: 2rem;
  }

  .closing-label {
    font-size: 0.95rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 1rem;
    opacity: 0.8;
  }

  .closing-text {
    font-size: 1.2rem;
    font-style: italic;
    color: var(--text-primary);
    line-height: 1.9;
    margin-top: 1.5rem;
  }
</style>
