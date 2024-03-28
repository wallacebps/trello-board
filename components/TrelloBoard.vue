<template>
  <div>
    <draggable
      v-model="columns"
      group="columns"
      :animation="150"
      handle=".drag-handle"
      item-key="id"
      class="flex gap-4 overflow-x-auto items-start"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 p-5 rounded min-w-[250px] mb-5">
          <header class="font-bold mb-4">
            <DragHandle />
            {{ column.title }}
          </header>
          <draggable
            v-model="column.tasks"
            :group="{name: 'tasks', pull: alt ? 'clone' : true}"
            :animation="150"
            handle=".drag-handle"
            item-key="id"
          >
            <template #item="{ element: task }: { element: Task }">
              <div>
                <TrelloBoardTask :task="task" @delete="column.tasks = column.tasks.filter(t => t.id !== $event)"/>
              </div>
            </template>
          </draggable>
          <footer>
            <NewTask @add="column.tasks.push($event)"/>
          </footer>
        </div>
      </template>
    </draggable>
  </div>
</template>

<script setup lang="ts">
import type { Column, Task } from "~/types";
import draggable from "vuedraggable";
import { nanoid } from "nanoid";

const columns = ref<Column[]>([
  {
    id: nanoid(),
    title: "Backlog",
    tasks: [
      {
        id: nanoid(),
        title: "Create landing page",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Fix type bugs",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Create new tests",
        createdAt: new Date(),
      },
    ],
  },
  {
    id: nanoid(),
    title: "Selected for Dev",
    tasks: [],
  },
  {
    id: nanoid(),
    title: "In Progress",
    tasks: [],
  },
  {
    id: nanoid(),
    title: "Code Review",
    tasks: [],
  },
  {
    id: nanoid(),
    title: "QA",
    tasks: [],
  },
  {
    id: nanoid(),
    title: "Complete",
    tasks: [],
  },
]);
const alt = useKeyModifier("Alt")
</script>

