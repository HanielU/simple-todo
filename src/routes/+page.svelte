<script lang="ts">
  import TodoItem from "./TodoItem.svelte";
  import cn from "clsx";
  import type { Todo } from "$lib/types";
  import { createSwitch, melt } from "@melt-ui/svelte";
  import { flip } from "svelte/animate";
  import { fade } from "svelte/transition";

  let text = "";
  let todos: Todo[] = [];

  const {
    elements: { root, input },
    states: { checked: showCompleted },
  } = createSwitch();

  function handleSubmit() {
    todos = [{ id: crypto.randomUUID(), text, completed: false }, ...todos];
    text = "";
  }
</script>

<div class="p-5">
  <div class="b-2 br-xl max-w-500px h-500px mx-a overflow-y-auto">
    <form class="p-4" on:submit|preventDefault={handleSubmit}>
      <input
        class="px py-2 bg-neutral-1 br-lg w-full outline-none"
        type="text"
        placeholder="Enter Todo"
        bind:value={text}
      />
    </form>

    <div class="p-4 flex-s-between text-sm fw-semibold">
      <h1 class="">Todos</h1>

      <div class="flex-s-center gap-2">
        <label class="lh-none" for="airplane-mode">Show Completed</label>
        <button
          use:melt={$root}
          class="relative h-6 w-11 br-full bg-neutral-2 transition-colors
          data-[state=checked]:bg-neutral-8"
          id="airplane-mode"
        >
          <span
            class={cn(
              "block square-5 br-full bg-white",
              "transition-transform will-change-transform",
              $showCompleted ? "translate-x-22px" : "translate-x-0.5"
            )}
          />
          <input use:melt={$input} />
        </button>
      </div>
    </div>

    <hr />

    <!-- TODOS LIST -->
    <ul class="p-4 flex flex-col gap-2">
      {#each todos.filter(v => {
        if ($showCompleted) {
          return v.completed === true;
        }
        return true;
      }) as todo (todo.id)}
        <li transition:fade={{ duration: 250 }} animate:flip={{ duration: 250 }}>
          <TodoItem
            {todo}
            on:toggleComplete={({ detail: { checked } }) => {
              todo.completed = checked;
              todos = todos;
            }}
          />
        </li>
      {/each}
    </ul>
  </div>
</div>
