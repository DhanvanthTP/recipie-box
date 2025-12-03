
<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  let isOpen = false;
  let userInput = '';
  let messages = [];

  const toggleChat = () => {
    isOpen = !isOpen;
    if (isOpen && messages.length === 0) {
        addBotMessage("Namaste! I'm your Indian recipe assistant. You can ask me for 'breakfast', 'lunch', 'dinner', or 'chaat' recipes.");
    }
  };

  const handleInput = (e) => {
    if (e.key === 'Enter') {
        sendMessage();
    }
  };

  const sendMessage = () => {
    if (!userInput.trim()) return;

    addUserMessage(userInput);
    processUserMessage(userInput);

    userInput = '';
  };
  
  const addUserMessage = (text) => {
      messages = [...messages, { text, sender: 'user' }];
  }

  const addBotMessage = (text) => {
       setTimeout(() => {
            messages = [...messages, { text, sender: 'bot' }];
       }, 500);
  }

  const processUserMessage = (text) => {
    const lowerText = text.toLowerCase();
    dispatch('filter', lowerText);

    let response = "I'm searching for Indian recipes related to that. Here is what I found!";

    if (lowerText.includes('hello') || lowerText.includes('hi') || lowerText.includes('namaste')) {
        response = "Hello! Tell me what kind of Indian dish you're looking for. Try 'breakfast' or 'curry'.";
    } else if (lowerText.includes('thank')) {
        response = "You're welcome! Enjoy your cooking.";
    } else if (lowerText.includes('help')) {
        response = "You can ask me to find recipes like 'breakfast', 'lunch', 'dinner', or 'chaat'. Just type in what you're craving!";
    } else if (lowerText.includes('all') || lowerText.includes('everything')) {
        response = "Showing all available Indian recipes!";
    }

    addBotMessage(response);
  };

</script>

<div class="chatbot-container">
    {#if isOpen}
    <div class="chat-window">
        <div class="chat-header">
            <h3>Recipe Assistant</h3>
        </div>
        <div class="chat-messages">
            {#each messages as message}
                <div class="message" class:user={message.sender === 'user'} class:bot={message.sender === 'bot'}>
                    <p>{message.text}</p>
                </div>
            {/each}
        </div>
        <div class="chat-input">
            <input 
                type="text" 
                placeholder="Ask for an Indian recipe..." 
                bind:value={userInput} 
                on:keydown={handleInput}
            />
            <button on:click={sendMessage}>Send</button>
        </div>
    </div>
    {/if}
    <button class="fab" on:click={toggleChat}>
         <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 20.94c-4.94-.46-8.5-4.52-8.5-9.44 0-5.24 4.26-9.5 9.5-9.5s9.5 4.26 9.5 9.5c0 .73-.09 1.44-.25 2.11"/><path d="M12 20.94c-4.94-.46-8.5-4.52-8.5-9.44 0-5.24 4.26-9.5 9.5-9.5s9.5 4.26 9.5 9.5c0 .73-.09 1.44-.25 2.11"/><path d="M12.5 15.5c-2 0-3.5-1.5-3.5-3.5s1.5-3.5 3.5-3.5 3.5 1.5 3.5 3.5c0 1.29-.68 2.42-1.7 3.06"/><path d="M12.5 15.5c-2 0-3.5-1.5-3.5-3.5s1.5-3.5 3.5-3.5 3.5 1.5 3.5 3.5c0 1.29-.68 2.42-1.7 3.06"/><path d="M12.5 15.5c-2 0-3.5-1.5-3.5-3.5s1.5-3.5 3.5-3.5 3.5 1.5 3.5 3.5c0 1.29-.68 2.42-1.7 3.06"/><path d="m12 15-2-2"/><path d="m12 15-2-2"/><path d="m12 15-2-2"/></svg>
    </button>
</div>

<style>
    .chatbot-container {
        position: fixed;
        bottom: 30px;
        right: 30px;
        z-index: 1000;
    }

    .fab {
        background: linear-gradient(145deg, #ff8c00, #ff4500);
        border: none;
        border-radius: 50%;
        width: 60px;
        height: 60px;
        color: white;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        box-shadow: 0 4px 15px rgba(255, 140, 0, 0.4);
        transition: all 0.3s ease;
    }

    .fab:hover {
        transform: scale(1.1);
        box-shadow: 0 6px 20px rgba(255, 140, 0, 0.6);
    }

    .chat-window {
        width: 350px;
        height: 450px;
        background-color: #2c2c2c;
        border-radius: 15px;
        margin-bottom: 20px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        display: flex;
        flex-direction: column;
        overflow: hidden;
        border: 1px solid rgba(255, 255, 255, 0.1);
    }

    .chat-header {
        background: #333;
        color: white;
        padding: 15px;
        text-align: center;
        font-weight: 600;
        border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .chat-messages {
        flex-grow: 1;
        padding: 15px;
        overflow-y: auto;
    }

    .message {
        margin-bottom: 10px;
        max-width: 85%;
    }

    .message p {
        margin: 0;
        padding: 10px 15px;
        border-radius: 20px;
        line-height: 1.5;
    }

    .message.bot {
        align-self: flex-start;
    }
    
    .message.bot p {
        background: #444;
        color: #f0f0f0;
    }

    .message.user {
       margin-left: auto;
    }

    .message.user p {
        background: #ff8c00;
        color: white;
        text-align: right;
    }

    .chat-input {
        display: flex;
        padding: 10px;
        border-top: 1px solid rgba(255, 255, 255, 0.1);
    }

    .chat-input input {
        flex-grow: 1;
        background: #333;
        border: none;
        padding: 10px;
        border-radius: 20px;
        color: white;
        margin-right: 10px;
    }
    
    .chat-input button {
        background: #ff4500;
        border: none;
        color: white;
        font-weight: 600;
        border-radius: 20px;
        padding: 0 15px;
        cursor: pointer;
        transition: background 0.3s;
    }

    .chat-input button:hover {
        background: #ff8c00;
    }
</style>
