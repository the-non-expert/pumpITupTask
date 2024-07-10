<script lang="ts">
  import { onMount } from "svelte";

  let isLoading = true;

  // Simulate a compute-intensive operation or data fetching
  function fakeCompute(): Promise<void> {
    return new Promise<void>((resolve) => {
      setTimeout(() => {
        isLoading = false;
        resolve();
      }, 3000); // Simulate a 3-second delay
    });
  }

  onMount(() => {
    fakeCompute();
  });

  function handleClick() {
    if (isLoading) {
      console.log("Button clicked too early!");
    } else {
      console.log("Button clicked successfully!");
    }
  }
</script>

<div class="flex items-center justify-center min-h-screen bg-gray-100">
  <div class="text-center">
    <h1 class="text-2xl font-bold mb-4">Page B</h1>
    
    <button
      on:click={handleClick}
      class="px-4 py-2 bg-green-500 text-white rounded">Click Me</button
    >
  </div>
</div>
