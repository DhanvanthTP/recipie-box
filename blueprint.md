# Project Blueprint

## Overview

This project is a static-first web application built with Astro.js. It is designed to be developed within the Firebase Studio (formerly Project IDX) environment. The focus is on creating a fast, highly-performant, and scalable site that delivers minimal JavaScript by default, ensuring an exceptional user experience and top-tier Core Web Vitals.

## Implemented Features

*   **Framework:** Astro.js
*   **UI:** Svelte Components
*   **Core Feature:** A voice-controlled recipe finder for a curated list of Indian dishes.
*   **Categorization:** Recipes are categorized by meal type (breakfast, lunch, dinner, chaat).
*   **AI Chatbot:** A floating chatbot that allows users to filter recipes via text commands.
*   **YouTube Summarizer:** A (simulated) feature to summarize recipes from YouTube links.
*   **Custom Cursor:** A unique, interactive cursor for enhanced user experience.
*   **Styling:** A modern, dark-themed UI with an Indian-inspired color palette.

## Current Request

**Request:** Add all the changes from the previous request, make the UI more colorful, and create a new icon for the other features.

**Plan:**

1.  **Enrich Recipe Data:** I will substantially update the `indianRecipes` array in `RecipeFinder.svelte`:
    *   **Detailed Descriptions:** Add more evocative and informative descriptions for each dish.
    *   **Comprehensive Ingredients:** Provide more complete and authentic ingredient lists.
    *   **Cooking Instructions:** Include a simplified, high-level overview of the cooking steps for each recipe within the description.
    *   **Expanded Keywords:** Add a much wider range of synonyms, regional names, and related terms to improve search accuracy (e.g., for Butter Chicken, add "Murgh Makhani").
2.  **Enhance Chatbot Intelligence (Simulation):** I will upgrade the `Chatbot.svelte` component to be a more knowledgeable and conversational assistant:
    *   **Contextual Responses:** Program the chatbot to recognize and answer common questions about Indian cuisine (e.g., "What is paneer?", "What is tandoori?").
    *   **Recommendation Logic:** Implement rules to provide recommendations based on user queries like "suggest a spicy dish" or "what is a popular breakfast?".
    *   **Improved Dialogue:** Refine the chatbot's conversational flow to feel more natural and helpful, guiding the user through their recipe discovery journey.
3.  **Create a "Features Hub":** I will create a new Svelte component, `FeaturesHub.svelte`, that will act as a central floating action button (FAB). When clicked, this FAB will expand to show separate icons for the AI Chatbot and the YouTube Summarizer. This will declutter the UI and provide a more organized user experience.
4.  **Colorful UI:** I will enhance the visual appeal of the application:
    *   **Textured Background:** I will add a subtle, textured background to the body of the application for a more premium feel.
    *   **Vibrant Colors:** I will introduce a more diverse and vibrant color palette for the recipe cards, buttons, and other interactive elements.
    *   **Gradients and Shadows:** I will make use of gradients and drop shadows to create a sense of depth and visual interest.
