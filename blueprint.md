# Project Blueprint

## Overview

This project is a static-first web application built with Astro.js. It is designed to be developed within the Firebase Studio (formerly Project IDX) environment. The focus is on creating a fast, highly-performant, and scalable site that delivers minimal JavaScript by default, ensuring an exceptional user experience and top-tier Core Web Vitals.

## Implemented Features

### Initial Setup

*   **Framework:** Astro.js
*   **UI Components:** Svelte
*   **Default Page:** `src/pages/index.astro`
*   **Default Component:** `src/components/Counter.svelte`
*   **Styling:** Basic global styles in `src/pages/index.astro`

## Current Request

**Request:** Use secure coding practices to create an error-free web app that enables users to speak their desired ingredients or a type of dish. The app will utilize generative AI to accurately transcribe and analyze the voice input, identify key components, and then generate a curated list of 3-5 suitable recipes. Each recipe in the list should clearly display its name, a concise list of primary ingredients, and a brief, appetizing description. If the voice input is unclear, or if no specific ingredients are identified, the app should instead generate a list of popular, easy-to-make dessert recipes. The user interface should include a prominent button to start and stop voice recording, along with a clear display area for the generated recipe list.

**Plan:**

1.  **Create the UI:** I will create a new Svelte component called `RecipeFinder.svelte` that will contain the record button and the recipe display area.
2.  **Implement Voice Recognition:** I will use the Web Speech API to capture and transcribe the user's voice input.
3.  **Simulate Generative AI:** I will simulate the generative AI functionality by creating a predefined set of recipes. If the user's speech contains keywords from a recipe, the app will display that recipe.
4.  **Fallback Recipes:** If no keywords are matched, a list of dessert recipes will be displayed.
5.  **Styling:** I will add modern styling to the component to ensure a good user experience.
6.  **Integration:** I will integrate the `RecipeFinder.svelte` component into the main `index.astro` page.
7.  **Cleanup:** I will remove the now unused `Counter.svelte` component.
