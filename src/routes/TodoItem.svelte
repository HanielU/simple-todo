<script lang="ts">
  import type { Todo } from "$lib/types";
  import { createCheckbox, melt } from "@melt-ui/svelte";
  import { createEventDispatcher } from "svelte";

  export let todo: Todo;
  const dispatch = createEventDispatcher<{
    toggleComplete: { checked: boolean };
  }>();

  const {
    elements: { root, input },
    helpers: { isChecked },
    states: { checked },
  } = createCheckbox({
    defaultChecked: false,
  });

  $: dispatch("toggleComplete", { checked: <boolean>$checked });
</script>

<div class="flex-s-start gap-2 bg-neutral-2 p-2 br-lg">
  <button class="square-18px flex-s-center br-md b-(2 neutral)" use:melt={$root}>
    {#if $isChecked}
      <div class="i-mdi-check text-(sm neutral-6)" />
    {/if}
    <input use:melt={$input} />
  </button>

  <span>{todo.text}</span>
</div>
