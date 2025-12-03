
<script>
  import { fly } from 'svelte/transition';

  let isOpen = false;

  const toggleMenu = () => {
    isOpen = !isOpen;
  };
</script>

<div class="features-hub-container">
  {#if isOpen}
    <div class="options" transition:fly={{ y: 20, duration: 300 }}>
      <button class="fab chatbot" on:click={() => dispatch('openChatbot')}>
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor"><path d="M12,2C6.48,2 2,6.48 2,12s4.48,10 10,10c1.85,0 3.57-0.5 5.06-1.38L21.59,22L19.62,18.62C21.08,17.03 22,14.64 22,12c0-5.52-4.48-10-10-10zM17,13H13v4h-2v-4H7v-2h4V7h2v4h4V13z"/></svg>
      </button>
      <button class="fab youtube" on:click={() => dispatch('openSummarizer')}>
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor"><path d="M10,15L15.19,12L10,9V15M21.56,7.17C21.69,7.64 21.78,8.27 21.84,9.07C21.91,9.87 21.94,10.56 21.94,11.16L22,12C22,14.19 21.84,15.8 21.56,16.83C21.31,17.73 20.73,18.31 19.83,18.56C19.36,18.69 18.73,18.78 17.93,18.84C17.13,18.91 16.44,18.94 15.84,18.94L12,19C9.81,19 8.2,18.84 7.17,18.56C6.27,18.31 5.69,17.73 5.44,16.83C5.31,16.36 5.22,15.73 5.16,14.93C5.09,14.13 5.06,13.44 5.06,12.84L5,12C5,9.81 5.16,8.2 5.44,7.17C5.69,6.27 6.27,5.69 7.17,5.44C7.64,5.31 8.27,5.22 9.07,5.16C9.87,5.09 10.56,5.06 11.16,5.06L12,5C14.19,5 15.8,5.16 16.83,5.44C17.73,5.69 18.31,6.27 18.56,7.17Z"/></path></svg>
      </button>
    </div>
  {/if}
  <button class="fab main" on:click={toggleMenu}>
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor"><path d="M12,2C6.48,2 2,6.48 2,12s4.48,10 10,10s10-4.48 10-10S17.52,2 12,2zM12,20c-4.41,0-8-3.59-8-8s3.59-8 8-8s8,3.59 8,8S16.41,20 12,20z"/><path d="M12,6c-3.31,0-6,2.69-6,6s2.69,6 6,6s6-2.69 6-6S15.31,6 12,6zM12,16c-2.21,0-4-1.79-4-4s1.79-4 4-4s4,1.79 4,4S14.21,16 12,16z"/></svg>
  </button>
</div>

<style>
  .features-hub-container {
    position: fixed;
    bottom: 30px;
    right: 30px;
    z-index: 1001;
    display: flex;
    flex-direction: column;
    align-items: flex-end;
  }

  .fab {
    border: none;
    border-radius: 50%;
    width: 60px;
    height: 60px;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease;
  }

  .fab:hover {
    transform: scale(1.1);
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
  }

  .fab.main {
    background: linear-gradient(145deg, #8a2be2, #4b0082);
  }

  .options {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    margin-bottom: 1rem;
  }

  .fab.chatbot {
    background: linear-gradient(145deg, #ff8c00, #ff4500);
  }

  .fab.youtube {
    background: linear-gradient(145deg, #ff4b4b, #ff0000);
  }
</style>
