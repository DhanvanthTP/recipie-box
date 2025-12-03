
<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  export let isOpen = false;
  let userInput = '';
  let messages = [];

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
    
    // Check for specific questions first
    if (lowerText.includes('paneer')) {
        addBotMessage("Paneer is a fresh, non-melting cheese common in Indian cuisine. It's made by curdling milk with a fruit or vegetable acid, like lemon juice. It has a mild, milky flavor and a dense, crumbly texture. I can show you some paneer dishes if you like!");
        return;
    } else if (lowerText.includes('tandoori') || lowerText.includes('tandoor')) {
        addBotMessage("Tandoori refers to a method of cooking in a 'tandoor,' a cylindrical clay oven. Food is cooked at high temperatures, which gives it a unique, smoky flavor. The famous 'Butter Chicken' uses tandoori chicken.");
        return;
    } else if (lowerText.includes('spicy') || lowerText.includes('hot')) {
        addBotMessage("If you like spicy food, I recommend trying Rogan Josh or Chole Bhature. Both are known for their delicious, hot flavors. Just say the name of the dish you want to see!");
        return;
    } else if (lowerText.includes('beginner') || lowerText.includes('easy') || lowerText.includes('simple')) {
        addBotMessage("Poha is a great dish for beginners! It's a quick and easy breakfast dish that doesn't require a lot of complicated steps. Palak Paneer is also a relatively simple curry to make.");
        return;
    } else if (lowerText.includes('mughlai')) {
        addBotMessage("Mughlai cuisine is a style of cooking developed in the Indian subcontinent by the imperial kitchens of the Mughal Empire. It is known for its richness and use of aromatic spices, nuts, and cream. Biryani is a classic example!");
        return;
    }

    // If no specific question, then filter recipes
    dispatch('filter', lowerText);

    let response = "I'm searching for Indian recipes related to that. Here is what I found!";

    if (lowerText.includes('hello') || lowerText.includes('hi') || lowerText.includes('namaste')) {
        response = "Hello! Tell me what kind of Indian dish you're looking for. Try 'breakfast' or 'curry'.";
    } else if (lowerText.includes('thank')) {
        response = "You're welcome! Enjoy your cooking.";
    } else if (lowerText.includes('help')) {
        response = "You can ask me to find recipes like 'breakfast', 'lunch', 'dinner', or 'chaat'. You can also ask me questions about Indian food!";
    } else if (lowerText.includes('all') || lowerText.includes('everything')) {
        response = "Showing all available Indian recipes!";
    }

    addBotMessage(response);
  };

</script>

{#if isOpen}
<div class="chatbot-container">
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
                placeholder="Ask about Indian food..." 
                bind:value={userInput} 
                on:keydown={handleInput}
            />
            <button on:click={sendMessage}>Send</button>
        </div>
    </div>
</div>
{/if}

<style>
    .chatbot-container {
        position: fixed;
        bottom: 110px; /* Adjusted to be above the features hub */
        right: 30px;
        z-index: 1000;
    }

    .chat-window {
        width: 350px;
        height: 450px;
        background-color: #2c2c2c;
        border-radius: 15px;
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
        display: flex;
    }

    .message p {
        margin: 0;
        padding: 10px 15px;
        border-radius: 20px;
        line-height: 1.5;
    }

    .message.bot {
        justify-content: flex-start;
    }
    
    .message.bot p {
        background: #444;
        color: #f0f0f0;
    }

    .message.user {
       justify-content: flex-end;
    }

    .message.user p {
        background: linear-gradient(145deg, #ff8c00, #ff4500);
        color: white;
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
