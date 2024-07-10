<script lang="ts">
  import { goto } from "$app/navigation";

  function navigateToPageB() {
    goto("/pageB");
  }
</script>

<div class="flex items-center justify-center min-h-screen bg-gray-100">
  <div class="text-center">
    <h1 class="text-2xl font-bold">Page A</h1>
    <p>This is a simulation of the home page for the user.</p>
    <button
      on:click={navigateToPageB}
      class="px-4 py-2 bg-blue-500 text-white rounded mt-5"
    >
      Go to Page B
    </button>
    <p>
      (There is a 3 second delay function on the second page as soon as it
      mounts to simulate a fake intensive server load.)
    </p>
  </div>
</div>
