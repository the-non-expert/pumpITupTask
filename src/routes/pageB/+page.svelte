<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import { writable } from "svelte/store";

  let isLoading = true;
  let capturing = true;
  let capturedEvents: MouseEvent[] = [];
  let eventsLog = writable<string[]>([]);

  // Simulate a compute-intensive operation or data fetching
  function fakeCompute(): Promise<void> {
    return new Promise<void>((resolve) => {
      setTimeout(() => {
        isLoading = false;
        resolve();
      }, 5000); // Simulate a 5-second delay
    });
  }

  function handleDocumentClick(event: MouseEvent) {
    if (capturing) {
      capturedEvents.push(event);
      eventsLog.update((log) => [
        ...log,
        `Captured click at (${event.clientX}, ${event.clientY})`,
      ]);
      event.preventDefault();
      event.stopImmediatePropagation();
    }
  }

  function resendCapturedEvents() {
    capturing = false;
    eventsLog.update((log) => [...log, "Replaying captured events..."]);
    for (const event of capturedEvents) {
      const target = document.elementFromPoint(
        event.clientX,
        event.clientY
      ) as HTMLElement;
      target?.dispatchEvent(new MouseEvent(event.type, event));
    }
    capturedEvents = [];
    eventsLog.update((log) => [...log, "All captured events replayed."]);
  }

  onMount(() => {
    if (typeof document !== "undefined") {
      document.addEventListener("click", handleDocumentClick, true);

      fakeCompute().then(() => {
        resendCapturedEvents();
      });
    }
  });

  onDestroy(() => {
    if (typeof document !== "undefined") {
      document.removeEventListener("click", handleDocumentClick, true);
    }
  });

  function handleClick() {
    eventsLog.update((log) => [...log, "Button click event processed."]);
  }
</script>

<div class="flex flex-col items-center justify-center min-h-screen bg-gray-100">
  <div class="text-center">
    <h1 class="text-2xl font-bold mb-4">Page B</h1>
    <button
      on:click={handleClick}
      class="px-4 py-2 bg-green-500 text-white rounded"
    >
      {"Click Me"}
    </button>
    <p class={isLoading ? "loading" : "ready"}>
      {isLoading
        ? "Page is still loading. Click Here to see the capturing of events before the page loads..."
        : "Page is ready. All events are being processed."}
    </p>
    <div class="event-log">
      <h2 class="font-bold mb-2">Event Log:</h2>
      <ul>
        {#each $eventsLog as logEntry}
          <li>{logEntry}</li>
        {/each}
      </ul>
    </div>
  </div>
</div>

<style>
  .loading {
    color: red;
  }
  .ready {
    color: green;
  }
  .event-log {
    margin-top: 20px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f9f9f9;
    max-height: 200px;
    overflow-y: auto;
  }
</style>
