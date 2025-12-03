
<script>
  import { onMount } from 'svelte';

  let recognition;
  let listening = false;
  let transcript = '';
  let recipes = [];
  let error = null;

  const allRecipes = [
    {
      name: 'Spaghetti Carbonara',
      ingredients: ['Spaghetti', 'Eggs', 'Parmesan Cheese', 'Pancetta', 'Black Pepper'],
      description: 'A classic Roman pasta dish that comes together in minutes. Creamy, savory, and deeply satisfying.',
      keywords: ['pasta', 'carbonara', 'spaghetti', 'italian']
    },
    {
      name: 'Chicken Tikka Masala',
      ingredients: ['Chicken', 'Yogurt', 'Tomato Sauce', 'Garam Masala', 'Cream'],
      description: 'A beloved Indian curry with tender chicken in a creamy, spiced tomato sauce. Perfect with naan or rice.',
      keywords: ['chicken', 'curry', 'indian', 'masala']
    },
    {
      name: 'Classic Beef Tacos',
      ingredients: ['Ground Beef', 'Taco Shells', 'Lettuce', 'Tomato', 'Cheese'],
      description: 'A fun, customizable meal for any night of the week. Everyone loves a good taco night!',
      keywords: ['tacos', 'beef', 'mexican', 'ground beef']
    },
    {
      name: 'Avocado Toast',
      ingredients: ['Bread', 'Avocado', 'Red Pepper Flakes', 'Lemon Juice', 'Salt'],
      description: 'A simple, trendy, and delicious breakfast or snack. Easy to customize with your favorite toppings.',
      keywords: ['avocado', 'toast', 'breakfast', 'vegetarian']
    },
     {
      name: 'Mushroom Risotto',
      ingredients: ['Arborio Rice', 'Mushrooms', 'Vegetable Broth', 'Onion', 'Parmesan Cheese'],
      description: 'A creamy and elegant Italian rice dish, perfect for a cozy dinner. The earthy mushrooms make it incredibly flavorful.',
      keywords: ['mushroom', 'risotto', 'rice', 'italian']
    }
  ];

  const dessertRecipes = [
    {
      name: 'Chocolate Chip Cookies',
      ingredients: ['Flour', 'Butter', 'Sugar', 'Chocolate Chips', 'Eggs'],
      description: 'The ultimate classic dessert. Warm, gooey, and impossible to resist. Perfect with a glass of milk.',
      keywords: ['dessert', 'cookies', 'chocolate']
    },
    {
      name: 'Classic Brownies',
      ingredients: ['Chocolate', 'Butter', 'Flour', 'Sugar', 'Eggs'],
      description: 'Fudgy, chewy, and intensely chocolatey. A guaranteed crowd-pleaser for any occasion.',
      keywords: ['dessert', 'brownies', 'chocolate']
    },
    {
      name: 'Apple Crumble',
      ingredients: ['Apples', 'Oats', 'Flour', 'Butter', 'Cinnamon'],
      description: 'A comforting and rustic dessert with warm, spiced apples under a crunchy oat topping. Serve with vanilla ice cream.',
      keywords: ['dessert', 'apple', 'crumble']
    }
  ];

  onMount(() => {
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
            error = "I didn't catch that. Please try again. Showing dessert recipes as a fallback.";
        } else {
            error = `Error during recognition: ${event.error}. Showing dessert recipes as a fallback.`;
        }
        recipes = dessertRecipes;
        listening = false;
      };

      recognition.onend = () => {
        listening = false;
      };
    } else {
      error = "Speech recognition is not supported in this browser. Please use Chrome or Safari.";
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
    const foundRecipes = allRecipes.filter(recipe => 
      recipe.keywords.some(kw => lowerText.includes(kw))
    );

    if (foundRecipes.length > 0) {
      recipes = foundRecipes.slice(0, 5);
    } else {
      recipes = dessertRecipes;
      error = "I couldn't find a match for that. Here are some popular dessert recipes instead!";
    }
  }
</script>

<main>
  <div class="container">
    <div class="title-container">
      <h1>Voice-to-Recipe</h1>
      <p>Speak your desired ingredients or dish, and let AI find your next meal.</p>
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
        <p><strong>You said:</strong> "{transcript}"</p>
      </div>
    {/if}

    {#if error}
        <div class="error-message">
            <p>{error}</p>
        </div>
    {/if}

    <div class="recipe-list">
      {#each recipes as recipe}
        <div class="recipe-card">
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
    text-shadow: 0 0 10px rgba(255, 255, 255, 0.3), 0 0 20px rgba(255, 105, 180, 0.5);
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
    background: linear-gradient(145deg, #ff69b4, #ff1493);
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
    box-shadow: 0 4px 15px rgba(255, 105, 180, 0.4);
    position: relative;
    overflow: hidden;
  }

  button::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(120deg, transparent, rgba(255, 255, 255, 0.3), transparent);
    transition: all 0.4s;
  }

  button:hover::before {
    left: 100%;
  }

  button:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba(255, 105, 180, 0.6);
  }

  button.listening {
    background: linear-gradient(145deg, #48c6ef, #6f86d6);
    box-shadow: 0 4px 15px rgba(72, 198, 239, 0.4);
  }

  .transcript, .error-message {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 10px;
    padding: 0.5rem 1rem;
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
    transform: translateY(-5px);
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
