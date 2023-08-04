<script lang="ts">
  import TodoItem from "./TodoItem.svelte";
  import cn from "clsx";
  import type { Todo } from "$lib/types";
  import { createSwitch, melt } from "@melt-ui/svelte";
  import { fade } from "svelte/transition";
  import { flip } from "svelte/animate";

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

    <div class="p-4 flex-s-between">
      <h1 class="text-sm fw-semibold">Todos</h1>

      <div class="flex-s-center gap-2">
        <button
          use:melt={$root}
          class="relative h-5 w-8 br-full bg-neutral-2 transition-colors
          flex-s-start data-[state=checked]:bg-neutral-8"
          id="completed"
        >
          <span
            class={cn(
              "block square-4 br-full bg-white",
              "transition-transform will-change-transform",
              $showCompleted ? "translate-x-14px" : "translate-x-0.5"
            )}
          />
          <input use:melt={$input} />
        </button>
        <label class="text-neutral-8" for="completed">Completed</label>
      </div>
    </div>

    <hr />

    <!-- TODOS LIST -->
    <ul class="p-4 flex flex-col gap-1">
      {#each todos.filter( v => ($showCompleted ? v.completed === true : true) ) as todo (todo.id)}
        <li
          transition:fade={{ duration: 250 }}
          animate:flip={{ duration: 250 }}
        >
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
