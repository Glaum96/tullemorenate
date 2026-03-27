<script>
  import PasswordPage from './lib/PasswordPage.svelte';
  import IntroSection from './lib/IntroSection.svelte';
  import MainContent from './lib/MainContent.svelte';
  import MusicToggle from './lib/MusicToggle.svelte';

  // 'password' → 'intro' → 'main'
  let appState = $state('password');

  function handleUnlock() {
    appState = 'intro';
  }

  function handleIntroDone() {
    appState = 'main';
  }
</script>

{#if appState === 'password'}
  <PasswordPage onUnlock={handleUnlock} />
{:else if appState === 'intro'}
  <IntroSection onDone={handleIntroDone} />
{:else}
  <MainContent />
{/if}

{#if appState !== 'password'}
  <MusicToggle />
{/if}
