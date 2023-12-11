<!-- scroll animations using svelte transitions and an observer -->

<script lang="ts">
  // importing utilities
  import { onMount } from "svelte";
  import { quintInOut } from 'svelte/easing';
  import { fly } from 'svelte/transition';

  // array to store visibility status of each sectionBlock
  let sectionVisibility: boolean[] = [];

  // transition handler
  function handleTransitions(index: number) {
    return (entries: IntersectionObserverEntry[]) => {
      // Update the visibility status for the corresponding section
      sectionVisibility[index] = entries[0].isIntersecting;
    };
  }

  // Lifecycle hook - runs after the component is first rendered to the DOM
  onMount(() => {
    // Select all elements with the class "sectionBlock"
    const sectionBlocks = document.querySelectorAll(".sectionBlock");
    
    // Loop through each section and set up an Intersection Observer
    sectionBlocks.forEach((section, index) => {
      // Create a new Intersection Observer for each section
      const sectionObserver = new IntersectionObserver(handleTransitions(index), {
        // represents the % of the element that has to be on screen
        threshold: 0.5,
      });

      // Start observing the current section
      sectionObserver.observe(section);

      // Initialize visibility status to false for each section
      sectionVisibility.push(false);
    });
  });
</script>

<!-- First section with a simple fly-in transition -->
<section class="sectionBlock">
  {#if sectionVisibility[0]}
    <div transition:fly={{ duration: 1000, x: 100, easing: quintInOut }}>
      Hello!
    </div>
  {/if}
</section>

<!-- Second section with a staged fly-in transition -->
<section class="sectionBlock">
  {#if sectionVisibility[1]}
    <!-- Each div has a separate fly-in transition -->
    <div transition:fly={{ duration: 1000, x: 100, easing: quintInOut }}>
      Yallo!
    </div>

    <div transition:fly={{ duration: 1100, x: 100, easing: quintInOut }}>
      Jello!
    </div>

    <div transition:fly={{ duration: 1200, x: 100, easing: quintInOut }}>
      Hallo!
    </div>
  {/if}
</section>

<!-- Custom styles for sections and divs -->
<!-- placeholders, use your own custom css instead! -->
<style>
  section {
    height: 100vh;
  }

  div {
    background-color: gray;
    padding: 1rem;
    margin: 1rem;
  }
</style>
