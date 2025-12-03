
<script>
  import { onMount } from 'svelte';
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
      ingredients: ['Flattened Rice', 'Onion', 'Peanuts', 'Turmeric', 'Lemon', 'Curry Leaves'],
      description: 'A light and fluffy breakfast dish from Maharashtra. To make, sauté onions, mustard seeds, and curry leaves. Add soaked poha, turmeric, and peanuts. Cook until heated through and garnish with fresh cilantro and a squeeze of lemon.',
      keywords: ['breakfast', 'poha', 'indian', 'vegetarian', 'quick', 'light', 'maharashtrian', 'flattened rice'],
      color: '#FFD700' // Gold
    },
    {
      name: 'Aloo Paratha',
      ingredients: ['Whole Wheat Flour', 'Potatoes', 'Garam Masala', 'Ghee', 'Cilantro', 'Green Chilies'],
      description: 'A popular North Indian breakfast of unleavened flatbread stuffed with a spiced potato mixture. Knead a soft dough. For the filling, mash boiled potatoes with garam masala, chilies, and cilantro. Roll out the dough, stuff with the filling, and pan-fry with ghee until golden brown.',
      keywords: ['breakfast', 'paratha', 'potato', 'aloo', 'indian', 'north indian', 'punjabi', 'vegetarian', 'flatbread'],
      color: '#FFA07A' // Light Salmon
    },
    {
      name: 'Masala Dosa',
      ingredients: ['Rice', 'Urad Dal', 'Potato', 'Onion', 'Turmeric', 'Sambar'],
      description: 'A crispy, savory crepe from South India, filled with a delicious spiced potato filling. The batter is made from fermented rice and lentils. The filling is a simple sauté of boiled potatoes, onions, and spices. Cook the dosa on a hot griddle until crisp and golden.',
      keywords: ['breakfast', 'dosa', 'masala dosa', 'south indian', 'vegetarian', 'crispy', 'fermented', 'gluten-free'],
      color: '#FF7F50' // Coral
    },

    // Lunch
    {
      name: 'Palak Paneer',
      ingredients: ['Spinach', 'Paneer', 'Onion', 'Tomato', 'Ginger-Garlic Paste', 'Cream'],
      description: 'A classic vegetarian dish with soft paneer cubes in a creamy, mildly spiced spinach gravy. Blanch and puree spinach. Sauté onions, ginger, and garlic, then add tomatoes and spices. Stir in the spinach puree and cream, then add paneer cubes and simmer.',
      keywords: ['lunch', 'dinner', 'palak paneer', 'paneer', 'spinach', 'vegetarian', 'curry', 'north indian', 'creamy'],
      color: '#3CB371' // Medium Sea Green
    },
    {
      name: 'Chole Bhature',
      ingredients: ['Chickpeas', 'All-Purpose Flour', 'Yogurt', 'Onion', 'Tomato', 'Spices'],
      description: 'A spicy chickpea curry served with fluffy, deep-fried bread. A hearty and beloved meal from Punjab. The chole (chickpea curry) is simmered in a tangy tomato-onion gravy. The bhature are made from a leavened dough, deep-fried until they puff up.',
      keywords: ['lunch', 'dinner', 'chole bhature', 'chickpea', 'punjabi', 'indian', 'spicy', 'fried bread'],
      color: '#CD853F' // Peru
    },
    {
      name: 'Biryani',
      ingredients: ['Basmati Rice', 'Chicken/Mutton/Veg', 'Yogurt', 'Saffron', 'Fried Onions', 'Mint'],
      description: 'A fragrant and flavorful rice dish with layers of marinated meat or vegetables, cooked with aromatic spices. Marinate the meat/veg in yogurt and spices. Layer it with partially cooked rice, fried onions, and mint. Cook on low heat (dum) until everything is tender and fragrant.',
      keywords: ['lunch', 'dinner', 'biryani', 'rice', 'chicken', 'mutton', 'indian', 'mughlai', 'dum'],
      color: '#B8860B' // Dark Goldenrod
    },

    // Dinner
    {
      name: 'Butter Chicken (Murgh Makhani)',
      ingredients: ['Chicken', 'Tomato Puree', 'Butter', 'Cream', 'Cashews', 'Fenugreek Leaves (Kasoori Methi)'],
      description: 'A rich and creamy curry with tender tandoori chicken in a buttery tomato sauce. A global favorite. Marinate and grill the chicken. The sauce is a smooth blend of tomato puree, cashew paste, butter, cream, and aromatic spices like kasoori methi.',
      keywords: ['dinner', 'butter chicken', 'murgh makhani', 'chicken', 'curry', 'punjabi', 'north indian', 'creamy', 'tandoori'],
      color: '#E9967A' // Dark Salmon
    },
    {
      name: 'Dal Makhani',
      ingredients: ['Black Lentils (Urad)', 'Kidney Beans (Rajma)', 'Butter', 'Cream', 'Tomato', 'Ginger'],
      description: 'A creamy, slow-cooked lentil dish from Punjab, known for its rich texture and flavor. The lentils and beans are simmered overnight, then cooked with tomato puree, ginger, garlic, and a generous amount of butter and cream for its characteristic richness.',
      keywords: ['dinner', 'dal makhani', 'dal', 'lentils', 'vegetarian', 'punjabi', 'creamy', 'slow-cooked'],
      color: '#8B4513' // Saddle Brown
    },
    {
      name: 'Rogan Josh',
      ingredients: ['Lamb/Mutton', 'Yogurt', 'Fennel Seeds', 'Ginger Powder', 'Kashmiri Red Chili'],
      description: 'An aromatic lamb curry from Kashmir with a rich, red gravy and fall-off-the-bone meat. The distinct red color comes from Kashmiri red chilies. The curry is flavored with aromatic spices like fennel and ginger powder, and has a yogurt-based gravy.',
      keywords: ['dinner', 'rogan josh', 'lamb', 'mutton', 'curry', 'kashmiri', 'spicy', 'aromatic'],
      color: '#A52A2A' // Brown
    },

    // Chaat (Snacks)
    {
      name: 'Pani Puri (Golgappa)',
      ingredients: ['Semolina (Sooji)', 'Potatoes', 'Chickpeas', 'Tamarind Chutney', 'Mint-Coriander Water'],
      description: 'A popular street food snack consisting of hollow, crispy balls filled with spicy tamarind water, potatoes, and chickpeas. It's a burst of flavors and textures in every bite! The fun is in eating them whole.',
      keywords: ['chaat', 'snack', 'pani puri', 'golgappa', 'street food', 'vegetarian', 'spicy', 'tangy'],
      color: '#4682B4' // Steel Blue
    },
    {
      name: 'Samosa Chaat',
      ingredients: ['Samosa', 'Yogurt', 'Tamarind Chutney', 'Mint Chutney', 'Onion', 'Sev'],
      description: 'Crushed samosas topped with chilled yogurt, tangy tamarind chutney, spicy mint chutney, onions, and crispy sev. It's a delicious and chaotic explosion of flavors, textures, and temperatures.',
      keywords: ['chaat', 'snack', 'samosa', 'street food', 'vegetarian', 'tangy', 'yogurt'],
      color: '#DAA520' // Goldenrod
    },
    {
      name: 'Bhel Puri',
      ingredients: ['Puffed Rice', 'Onion', 'Tomato', 'Tamarind Chutney', 'Potatoes', 'Sev'],
      description: 'A savory and tangy snack made with puffed rice, chopped onions, potatoes, tomatoes, and a mix of sweet and spicy chutneys. It is a light, crunchy, and refreshing snack, perfect for evenings.',
      keywords: ['chaat', 'snack', 'bhel puri', 'street food', 'vegetarian', 'light', 'puffed rice', 'mumbai'],
      color: '#F4A460' // Sandy Brown
    }
];
\
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

    const foundRecipes = allRecipesMaster.filter(recipe =>       recipe.keywords.some(kw => lowerText.includes(kw)) || recipe.name.toLowerCase().includes(lowerText)
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
    background: rgba(0, 0, 0, 0.3);
    border-radius: 10px;
    padding: 1rem 1.5rem;
    margin: 1.5rem auto;
    max-width: 90%;
    backdrop-filter: blur(5px);
    border: 1px solid rgba(255, 255, 255, 0.1);
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
    background: linear-gradient(145deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.05));
    border-radius: 15px;
    padding: 2rem;
    text-align: left;
    border: 1px solid rgba(255, 255, 255, 0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
    border-left: 5px solid var(--card-color, #ff8c00);
  }

  .recipe-card:hover {
    transform: translateY(-5px) scale(1.02);
    box-shadow: 0 10px 40px rgba(0,0,0,0.4);
  }

  .recipe-card h2 {
    font-size: 1.8rem;
    color: var(--card-color, #ffb6c1);
    margin-top: 0;
    margin-bottom: 1rem;
    border-bottom: 2px solid var(--card-color, #ffb6c1);
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
      background: rgba(255, 255, 255, 0.1);
      color: #ddd;
      padding: 5px 10px;
      border-radius: 20px;
      font-size: 0.9rem;
  }

  .description {
    color: #c0c0c0;
    line-height: 1.6;
  }
</style>
