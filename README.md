# Goal

The goal of this exercise is to reproduce a flaw in SvelteKit where fast clicking on a page button may not work due to listeners
not being fully loaded yet. This issue can occur when there are compute-intensive or data-loading functions to run between the
page being displayed and the page becoming fully functional with all listeners active.

# Flaw Description

When a page loads, it is displayed earlier than listeners on components are actually registered. If there are compute-intensive
or data-loading functions to run between the page being displayed and the page becoming fully functional with all listeners
active, a fast-clicking user may experience that clicking on a button very fast will not work. The reason is that there is not
yet any listener on the button (e.g., on:click).

## Implementation Details

### Step 1: Creating Page A which serves as the home page and adding a navigation button to Page B

### Step 2: Setting Up Page B with Event Capture and Fake Compute

**File: src/routes/pageB.svelte**

The logic in pageB.svelte includes:

- Capturing click events before the page fully loads.

- Replaying those captured events once the page has fully loaded.
