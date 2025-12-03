# Project Blueprint

## Overview

This project is a static-first web application built with Astro.js. It is designed to be developed within the Firebase Studio (formerly Project IDX) environment. The focus is on creating a fast, highly-performant, and scalable site that delivers minimal JavaScript by default, ensuring an exceptional user experience and top-tier Core Web Vitals.

## Implemented Features

*   **Framework:** Astro.js
*   **UI:** Svelte Components
*   **Core Feature:** A voice-controlled recipe finder.
*   **Voice Recognition:** Uses the Web Speech API to transcribe user voice input.
*   **AI Chatbot:** A floating chatbot that allows users to filter recipes via text commands.
*   **YouTube Summarizer:** A (simulated) feature to summarize recipes from YouTube links.
*   **Custom Cursor:** A unique, interactive cursor for enhanced user experience.
*   **Styling:** Modern, dark-themed UI with gradient buttons, card-based layout, and custom fonts.

## Current Request

**Request:** Replace the existing recipes with a curated list of popular Indian dishes, categorized by meals like breakfast, lunch, dinner, and chaats (savory snacks).

**Plan:**

1.  **Update Recipes:** I will replace the existing `allRecipes` array in `RecipeFinder.svelte` with a new, comprehensive list of popular Indian dishes.
2.  **Categorization:** I will add specific keywords to each new recipe to categorize them into "breakfast", "lunch", "dinner", and "chaat".
3.  **Update Chatbot:** I will modify the initial greeting message in `Chatbot.svelte` to inform the user about the new Indian recipe categories they can search for.
4.  **Refine Search:** I will ensure the search logic in `RecipeFinder.svelte` can effectively use the new categories and keywords.
