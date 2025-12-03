<script>
  import RecipeFinder from './RecipeFinder.svelte';
  import Chatbot from './Chatbot.svelte';
  import YouTubeSummarizer from './YouTubeSummarizer.svelte';
  import FeaturesHub from './FeaturesHub.svelte';

  let isChatbotOpen = false;
  let isSummarizerOpen = false;
  let recipeFinder;

  const handleOpenChatbot = () => {
    isChatbotOpen = !isChatbotOpen;
    isSummarizerOpen = false;
  };

  const handleOpenSummarizer = () => {
    isSummarizerOpen = !isSummarizerOpen;
    isChatbotOpen = false;
  };

  const handleFilter = (e) => {
    if (recipeFinder) {
      recipeFinder.filterFromChat(e.detail);
    }
  };
</script>

<main>
  <RecipeFinder bind:this={recipeFinder} />
  <FeaturesHub on:openChatbot={handleOpenChatbot} on:openSummarizer={handleOpenSummarizer} />
  <Chatbot bind:isOpen={isChatbotOpen} on:filter={handleFilter} />
  <YouTubeSummarizer bind:isOpen={isSummarizerOpen} />
</main>
