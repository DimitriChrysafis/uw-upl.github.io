<script>
  import { onMount } from "svelte";
  import Icon from "@iconify/svelte";

  let isOpen = null;
  let cstDate = "";
  let error = false;

  onMount(async () => {
    try {
      const res = await fetch("https://doors.amoses.dev/door-status");
      const data = await res.json();

      isOpen = data.status === "open";
      const timestamp = new Date(data.last_updated);

      cstDate = timestamp.toLocaleString("en-US", {
        timeZone: "America/Chicago",
        year: "numeric",
        month: "2-digit",
        day: "2-digit",
        hour: "2-digit",
        minute: "2-digit",
        hour12: true,
      });
    } catch {
      error = true;
    }
  });
</script>

<div
  style="display: flex; align-items: center; justify-content: center; gap: 0.1rem;"
>
  <p style="margin: 0;">
    {#if error}
      Error loading door status.
    {:else if isOpen === null}
      Loading door status...
    {:else if isOpen}
      ...is <span style="color: green; font-weight: 600;">open</span> right now (since {cstDate})!
    {:else}
      ...is <span style="color: rgb(183, 1, 1); font-weight: 600;">closed</span> (since {cstDate})
    {/if}
  </p>
  {#if isOpen}
    <Icon icon="material-symbols:door-open-outline-rounded" />
  {:else}
    <Icon icon="material-symbols:door-front-outline-rounded" />
  {/if}
</div>
