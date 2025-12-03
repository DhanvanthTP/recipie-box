
<script>
  import { onMount, onDestroy } from 'svelte';

  let x = 0;
  let y = 0;
  let dotSize = 8;
  let outlineSize = 40;
  let isPointer = false;

  const updatePosition = (e) => {
    x = e.clientX;
    y = e.clientY;
  };

  const updateCursorState = (e) => {
    const target = e.target;
    isPointer = window.getComputedStyle(target).getPropertyValue('cursor') === 'pointer';
  };

  onMount(() => {
    window.addEventListener('mousemove', updatePosition);
    document.addEventListener('mouseover', updateCursorState, true);
    return () => {
      window.removeEventListener('mousemove', updatePosition);
      document.removeEventListener('mouseover', updateCursorState, true);
    };
  });
</script>

<div 
  class="cursor-dot" 
  style="transform: translate({x - dotSize / 2}px, {y - dotSize / 2}px);"
></div>
<div 
  class="cursor-outline" 
  style="transform: translate({x - outlineSize / 2}px, {y - outlineSize / 2}px); width: {outlineSize}px; height: {outlineSize}px;"
  class:pointer={isPointer}
></div>

<style>
  .cursor-dot {
    position: fixed;
    top: 0;
    left: 0;
    width: 8px;
    height: 8px;
    background-color: #ff69b4;
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    transition: transform 0.1s ease-out;
  }

  .cursor-outline {
    position: fixed;
    top: 0;
    left: 0;
    width: 40px;
    height: 40px;
    border: 2px solid #ff69b4;
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    transition: all 0.2s ease-out;
  }
  
  .cursor-outline.pointer {
      transform-origin: center center;
      transform: scale(1.5);
      border-color: #48c6ef;
  }

  /* Hide the default cursor */
  :global(body, a, button) {
    cursor: none;
  }
</style>
