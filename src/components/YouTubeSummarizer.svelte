
<script>
  let isOpen = false;
  let url = '';
  let summary = '';
  let error = '';

  const toggleSummarizer = () => {
    isOpen = !isOpen;
    url = '';
    summary = '';
    error = '';
  };

  const handleSubmit = () => {
    if (url.includes('youtube.com') || url.includes('youtu.be')) {
      error = '';
      summary = 'Feature in development: Imagine a concise summary of your recipe video appearing here, with all the key steps and ingredients listed out for you!';
    } else {
      summary = '';
      error = 'Please enter a valid YouTube URL.';
    }
  };
</script>

<div class="summarizer-container">
  {#if isOpen}
    <div class="summarizer-window">
      <div class="summarizer-header">
        <h3>YouTube Recipe Summarizer</h3>
      </div>
      <div class="summarizer-content">
        <p>Paste a YouTube link to get a summarized recipe. (Demonstration only)</p>
        <div class="input-group">
          <input type="text" bind:value={url} placeholder="e.g., https://www.youtube.com/watch?v=...">
          <button on:click={handleSubmit}>Summarize</button>
        </div>
        {#if summary}
          <div class="summary-output">
            <p>{summary}</p>
          </div>
        {/if}
        {#if error}
          <div class="error-output">
            <p>{error}</p>
          </div>
        {/if}
      </div>
    </div>
  {/if}
  <button class="fab youtube" on:click={toggleSummarizer}>
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor"><path d="M10,15L15.19,12L10,9V15M21.56,7.17C21.69,7.64 21.78,8.27 21.84,9.07C21.91,9.87 21.94,10.56 21.94,11.16L22,12C22,14.19 21.84,15.8 21.56,16.83C21.31,17.73 20.73,18.31 19.83,18.56C19.36,18.69 18.73,18.78 17.93,18.84C17.13,18.91 16.44,18.94 15.84,18.94L12,19C9.81,19 8.2,18.84 7.17,18.56C6.27,18.31 5.69,17.73 5.44,16.83C5.31,16.36 5.22,15.73 5.16,14.93C5.09,14.13 5.06,13.44 5.06,12.84L5,12C5,9.81 5.16,8.2 5.44,7.17C5.69,6.27 6.27,5.69 7.17,5.44C7.64,5.31 8.27,5.22 9.07,5.16C9.87,5.09 10.56,5.06 11.16,5.06L12,5C14.19,5 15.8,5.16 16.83,5.44C17.73,5.69 18.31,6.27 18.56,7.17Z"></path></svg>
  </button>
</div>

<style>
  .summarizer-container {
    position: fixed;
    bottom: 30px;
    right: 110px; /* Positioned to the left of the chatbot */
    z-index: 1000;
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
    transition: all 0.3s ease;
  }

  .fab.youtube {
    background: linear-gradient(145deg, #ff4b4b, #ff0000);
    box-shadow: 0 4px 15px rgba(255, 0, 0, 0.4);
  }

  .fab.youtube:hover {
    transform: scale(1.1);
    box-shadow: 0 6px 20px rgba(255, 0, 0, 0.6);
  }
  
  .summarizer-window {
    width: 350px;
    background-color: #2c2c2c;
    border-radius: 15px;
    margin-bottom: 20px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.3);
    overflow: hidden;
    border: 1px solid rgba(255, 255, 255, 0.1);
  }

  .summarizer-header {
    background: #333;
    color: white;
    padding: 15px;
    text-align: center;
    font-weight: 600;
  }

  .summarizer-content {
    padding: 20px;
  }
  .summarizer-content p {
    color: #b0b0b0;
    text-align: center;
    margin-top: 0;
  }

  .input-group {
    display: flex;
    margin-top: 1rem;
  }

  .input-group input {
    flex-grow: 1;
    background: #333;
    border: 1px solid #555;
    padding: 10px;
    border-radius: 20px 0 0 20px;
    color: white;
  }

  .input-group button {
    background: #ff0000;
    border: none;
    color: white;
    font-weight: 600;
    border-radius: 0 20px 20px 0;
    padding: 0 15px;
    cursor: pointer;
  }

  .summary-output, .error-output {
    margin-top: 1rem;
    padding: 1rem;
    border-radius: 10px;
  }
  
  .summary-output {
      background: rgba(0, 255, 0, 0.1);
      color: #90ee90;
  }

  .error-output {
      background: rgba(255, 0, 0, 0.1);
      color: #ffcccc;
  }
</style>
