<script lang="ts">
  import cn from "clsx";
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

<div class="flex-s-start gap-2 bg-neutral-1 p-4 br-lg">
  <button
    class={cn(
      "square-12px flex-s-center br-4px b-(2 neutral)",
      $isChecked && "bg-neutral-8"
    )}
    use:melt={$root}
    id={todo.id}
  >
    <input use:melt={$input} />
  </button>

  <label class="text-15px/1rem fw-medium" for={todo.id}>{todo.text}</label>
</div>
