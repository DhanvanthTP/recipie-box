# Project Blueprint

## Overview

This project is a static-first web application built with Astro.js. It is designed to be developed within the Firebase Studio (formerly Project IDX) environment. The focus is on creating a fast, highly-performant, and scalable site that delivers minimal JavaScript by default, ensuring an exceptional user experience and top-tier Core Web Vitals.

## Implemented Features

*   **Framework:** Astro.js
*   **UI:** Svelte Components
*   **Core Feature:** A voice-controlled recipe finder.
*   **Voice Recognition:** Uses the Web Speech API to transcribe user voice input.
*   **Simulated AI:** Matches keywords from voice input to a predefined list of recipes.
*   **Fallback:** Displays dessert recipes if no match is found.
*   **Component:** `src/components/RecipeFinder.svelte` handles all core logic and UI.
*   **Styling:** Modern, dark-themed UI with gradient buttons, card-based layout, and custom fonts.

## Current Request

**Request:** Make the UI extremely interactive, ensure the cursor is very catchy, and add an AI chatbot that helps you to navigate through the recipes.

**Plan:**

1.  **Custom Cursor:** I will create a `CustomCursor.svelte` component to implement a visually engaging cursor that follows the user's mouse, with effects to make it stand out.
2.  **AI Chatbot:** 
    *   I will create a `Chatbot.svelte` component featuring a floating action button that opens a chat interface.
    *   This chatbot will be simulated. It will recognize keywords in the user's text input (e.g., "pasta", "dessert", "show me chicken recipes").
    *   Based on the keywords, the chatbot will provide responses and programmatically filter the recipe list displayed in the main `RecipeFinder` component.
3.  **Enhanced Interactivity:**
    *   I will add subtle animations and transitions to the recipe cards and buttons to make them feel more responsive and engaging.
    *   I will refine the visual feedback for user actions, such as when the voice recognition is active.
4.  **Component Communication:** I will use Svelte's component event system to allow the `Chatbot` to communicate the user's search query to the `RecipeFinder` component.
5.  **Integration:** I will update `index.astro` to include the new `CustomCursor` and `Chatbot` components, ensuring they are loaded for client-side interactivity.
