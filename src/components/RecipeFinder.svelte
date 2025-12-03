
<script>
  import { onMount, createEventDispatcher } from 'svelte';
  import { fly, fade } from 'svelte/transition';

  let recognition;
  let listening = false;
  let transcript = '';
  let recipes = [];
  let allRecipesMaster = [];
  let error = null;

  const indianRecipes = [
    // Breakfast
    {
      name: 'Poha',
      ingredients: ['Flattened Rice', 'Onion', 'Peanuts', 'Turmeric', 'Lemon'],
      description: 'A light and fluffy breakfast dish from Maharashtra, made with flattened rice, spices, and peanuts.',
      keywords: ['breakfast', 'poha', 'indian', 'vegetarian', 'quick', 'light']
    },
    {
      name: 'Aloo Paratha',
      ingredients: ['Whole Wheat Flour', 'Potatoes', 'Spices', 'Ghee', 'Cilantro'],
      description: 'A popular North Indian breakfast of unleavened flatbread stuffed with a spiced potato mixture.',
      keywords: ['breakfast', 'paratha', 'potato', 'aloo', 'indian', 'north indian', 'vegetarian']
    },
    {
      name: 'Masala Dosa',
      ingredients: ['Rice', 'Lentils', 'Potato', 'Onion', 'Spices'],
      description: 'A crispy, savory crepe from South India, filled with a delicious spiced potato filling.',
      keywords: ['breakfast', 'dosa', 'masala dosa', 'south indian', 'vegetarian', 'crispy']
    },

    // Lunch
    {
      name: 'Palak Paneer',
      ingredients: ['Spinach', 'Paneer', 'Onion', 'Tomato', 'Spices'],
      description: 'A classic vegetarian dish with soft paneer cubes in a creamy, mildly spiced spinach gravy.',
      keywords: ['lunch', 'dinner', 'palak paneer', 'paneer', 'spinach', 'vegetarian', 'curry']
    },
    {
      name: 'Chole Bhature',
      ingredients: ['Chickpeas', 'Flour', 'Yogurt', 'Spices', 'Tomato'],
      description: 'A spicy chickpea curry served with fluffy, deep-fried bread. A hearty and beloved meal from Punjab.',
      keywords: ['lunch', 'dinner', 'chole bhature', 'chickpea', 'punjabi', 'indian']
    },
    {
      name: 'Biryani',
      ingredients: ['Basmati Rice', 'Chicken/Mutton/Veg', 'Yogurt', 'Saffron', 'Spices'],
      description: 'A fragrant and flavorful rice dish with layers of meat or vegetables, cooked with aromatic spices.',
      keywords: ['lunch', 'dinner', 'biryani', 'rice', 'chicken', 'mutton', 'indian']
    },

    // Dinner
    {
      name: 'Butter Chicken',
      ingredients: ['Chicken', 'Tomato', 'Butter', 'Cream', 'Cashews'],
      description: 'A rich and creamy curry with tender tandoori chicken in a buttery tomato sauce. A global favorite.',
      keywords: ['dinner', 'butter chicken', 'chicken', 'curry', 'punjabi', 'north indian']
    },
    {
      name: 'Dal Makhani',
      ingredients: ['Black Lentils', 'Kidney Beans', 'Butter', 'Cream', 'Tomato'],
      description: 'A creamy, slow-cooked lentil dish from Punjab, known for its rich texture and flavor.',
      keywords: ['dinner', 'dal makhani', 'dal', 'lentils', 'vegetarian', 'punjabi']
    },
    {
      name: 'Rogan Josh',
      ingredients: ['Lamb/Mutton', 'Yogurt', 'Fennel Seeds', 'Ginger', 'Spices'],
      description: 'An aromatic lamb curry from Kashmir with a rich, red gravy and fall-off-the-bone meat.',
      keywords: ['dinner', 'rogan josh', 'lamb', 'mutton', 'curry', 'kashmiri']
    },

    // Chaat (Snacks)
    {
      name: 'Pani Puri / Golgappa',
      ingredients: ['Semolina', 'Potatoes', 'Chickpeas', 'Tamarind', 'Mint'],
      description: 'A popular street food snack consisting of hollow, crispy balls filled with spicy tamarind water and fillings.',
      keywords: ['chaat', 'snack', 'pani puri', 'golgappa', 'street food', 'vegetarian']
    },
    {
      name: 'Samosa Chaat',
      ingredients: ['Samosa', 'Yogurt', 'Tamarind Chutney', 'Mint Chutney', 'Onion'],
      description: 'Crushed samosas topped with yogurt, chutneys, and spices. A delicious and tangy explosion of flavors.',
      keywords: ['chaat', 'snack', 'samosa', 'street food', 'vegetarian']
    },
    {
      name: 'Bhel Puri',
      ingredients: ['Puffed Rice', 'Onion', 'Tomato', 'Tamarind Chutney', 'Sev'],
      description: 'A savory and tangy snack made with puffed rice, vegetables, and a mix of chutneys.',
      keywords: ['chaat', 'snack', 'bhel puri', 'street food', 'vegetarian', 'light']
    }
];


  const dessertRecipes = indianRecipes.filter(r => r.keywords.includes('dessert')); // Will be empty now, but logic remains

  onMount(() => {
    allRecipesMaster = [...indianRecipes];
    recipes = allRecipesMaster;
    
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (SpeechRecognition) {
      recognition = new SpeechRecognition();
      recognition.continuous = false;
      recognition.lang = 'en-US';

      recognition.onstart = () => {
        listening = true;
        transcript = '';
        error = null;
      };

      recognition.onresult = (event) => {
        transcript = event.results[0][0].transcript;
        findRecipes(transcript);
      };

      recognition.onerror = (event) => {
        if (event.error === 'no-speech') {
            error = "I didn't catch that. Please try again.";
        } else {
            error = `Error during recognition: ${event.error}.`;
        }
        listening = false;
      };

      recognition.onend = () => {
        listening = false;
      };
    } else {
      error = "Speech recognition is not supported in this browser.";
    }
  });

  function toggleListen() {
    if (listening) {
      recognition.stop();
    } else {
      recognition.start();
    }
  }

  function findRecipes(text) {
    const lowerText = text.toLowerCase();
    
    if (lowerText.includes('all') || lowerText.includes('everything')) {
        recipes = allRecipesMaster;
        error = null;
        return;
    }

    const foundRecipes = allRecipesMaster.filter(recipe => 
      recipe.keywords.some(kw => lowerText.includes(kw))
    );

    if (foundRecipes.length > 0) {
      recipes = foundRecipes;
      error = null;
    } else {
      recipes = [];
      error = "Sorry, I couldn't find any Indian dishes matching your search. Try asking for 'breakfast', 'lunch', 'dinner', or 'chaat'.";
    }
  }

  // This function will be called from the parent component (index.astro)
  export const filterFromChat = (text) => {
      transcript = text; // Update transcript to show what was searched
      findRecipes(text);
  }

</script>

<main>
  <div class="container">
    <div class="title-container">
      <h1>Indian Recipe Finder</h1>
      <p>Your AI assistant for discovering authentic Indian cuisine.</p>
    </div>

    <div class="button-container">
      <button on:click={toggleListen} class:listening>
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
          <path d="M12 14c1.66 0 2.99-1.34 2.99-3L15 5c0-1.66-1.34-3-3-3S9 3.34 9 5v6c0 1.66 1.34 3 3 3zm5.3-3c0 3-2.54 5.1-5.3 5.1S6.7 14 6.7 11H5c0 3.41 2.72 6.23 6 6.72V21h2v-3.28c3.28-.49 6-3.31 6-6.72h-1.7z"/>
        </svg>
        <span>{listening ? 'Listening...' : 'Tap to Speak'}</span>
      </button>
    </div>

    {#if transcript}
      <div class="transcript">
        <p><strong>Searched for:</strong> "{transcript}"</p>
      </div>
    {/if}

    {#if error}
        <div class="error-message">
            <p>{error}</p>
        </div>
    {/if}

    <div class="recipe-list">
      {#each recipes as recipe (recipe.name)}
        <div class="recipe-card" in:fly={{ y: 20, duration: 300 }} out:fade={{ duration: 200 }}>
          <h2>{recipe.name}</h2>
          <div class="ingredients">
            <strong>Primary Ingredients:</strong>
            <ul>
                {#each recipe.ingredients as ingredient}
                    <li>{ingredient}</li>
                {/each}
            </ul>
          </div>
          <p class="description">{recipe.description}</p>
        </div>
      {/each}
    </div>
     {#if recipes.length === 0 && !error}
        <div class="no-results">
            <p>Explore recipes by speaking or using the chatbot. Try saying "Show me breakfast" or "Find chicken dishes".</p>
        </div>
    {/if}
  </div>
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');

  :global(body) {
    background-color: #1a1a1a;
    color: #f0f0f0;
    font-family: 'Poppins', sans-serif;
    margin: 0;
    padding: 2rem;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  
  .container {
    max-width: 800px;
    margin: 0 auto;
    text-align: center;
    backdrop-filter: blur(10px);
    background-color: rgba(255, 255, 255, 0.05);
    border-radius: 20px;
    padding: 2rem;
    border: 1px solid rgba(255, 255, 255, 0.1);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
  }

  .title-container h1 {
    font-size: 3.5rem;
    font-weight: 700;
    color: #ffffff;
    margin-bottom: 0.5rem;
    text-shadow: 0 0 10px rgba(255, 165, 0, 0.5), 0 0 20px rgba(255, 69, 0, 0.5);
  }

  .title-container p {
    font-size: 1.2rem;
    color: #b0b0b0;
    margin-bottom: 2rem;
  }

  .button-container {
    margin-bottom: 2rem;
  }

  button {
    background: linear-gradient(145deg, #ff8c00, #ff4500);
    border: none;
    border-radius: 50px;
    color: white;
    padding: 15px 30px;
    font-size: 1.2rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    display: inline-flex;
    align-items: center;
    gap: 10px;
    box-shadow: 0 4px 15px rgba(255, 140, 0, 0.4);
    position: relative;
    overflow: hidden;
  }

  button:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba(255, 140, 0, 0.6);
  }

  button.listening {
    background: linear-gradient(145deg, #48c6ef, #6f86d6);
    box-shadow: 0 4px 15px rgba(72, 198, 239, 0.4);
  }

  .transcript, .error-message, .no-results {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 10px;
    padding: 1rem 1.5rem;
    margin: 1.5rem auto;
    max-width: 90%;
  }

  .error-message {
      color: #ffcccc;
      background: rgba(255, 0, 0, 0.2);
  }
  
  .recipe-list {
    display: grid;
    grid-template-columns: 1fr;
    gap: 1.5rem;
    margin-top: 2rem;
  }

  .recipe-card {
    background: rgba(255, 255, 255, 0.08);
    border-radius: 15px;
    padding: 2rem;
    text-align: left;
    border: 1px solid rgba(255, 255, 255, 0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .recipe-card:hover {
    transform: translateY(-5px) scale(1.02);
    box-shadow: 0 10px 20px rgba(0,0,0,0.2);
  }

  .recipe-card h2 {
    font-size: 1.8rem;
    color: #ffb6c1;
    margin-top: 0;
    margin-bottom: 1rem;
    border-bottom: 2px solid #ffb6c1;
    padding-bottom: 0.5rem;
  }
  
  .ingredients {
      margin-bottom: 1rem;
  }

  .ingredients strong {
      color: #e0e0e0;
      font-weight: 600;
  }

  .ingredients ul {
      list-style-type: none;
      padding: 0;
      margin-top: 0.5rem;
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem;
  }

  .ingredients li {
      background: rgba(255, 182, 193, 0.2);
      color: #ffb6c1;
      padding: 5px 10px;
      border-radius: 20px;
      font-size: 0.9rem;
  }

  .description {
    color: #c0c0c0;
    line-height: 1.6;
  }
</style>
