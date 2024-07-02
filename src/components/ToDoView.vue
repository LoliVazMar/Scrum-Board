<script setup>
import { reactive } from 'vue'
import NewTask from './NewTask.vue'

let boards = reactive([
  {
    id: crypto.randomUUID(),
    name: 'ToDo',
    items: [{ id: crypto.randomUUID(), title: 'Tarea 1' }]
  },
  {
    id: crypto.randomUUID(),
    name: 'In progress',
    items: [{ id: crypto.randomUUID(), title: 'Tarea 2' }]
  },
  {
    id: crypto.randomUUID(),
    name: 'Done',
    items: [{ id: crypto.randomUUID(), title: 'Tarea 3' }]
  }
])

function handleNewItem(text, board) {
  board.items.push({ id: crypto.randomUUID(), title: text.value })
}

function createNewBoard() {
  const name = prompt('Name of board')
  if (name) {
    const board = {
      id: crypto.randomUUID(),
      name: name,
      items: []
    }

    boards.push(board)
  }
}

function startDrag(evt, boardId, itemId) {
  console.log(boardId, itemId)
  evt.dataTransfer.dropEffect = 'move'
  evt.dataTransfer.effectAllowed = 'move'
  evt.dataTransfer.setData('item', JSON.stringify({ boardId, itemId }))
}

function onDrop(evt, dest) {
  const { boardId, itemId } = JSON.parse(evt.dataTransfer.getData('item'))
  console.log({ boardId, itemId })
  const board = boards.find((board) => board.id === boardId)
  const item = board.items.find((item) => item.id === itemId)
  board.items = board.items.filter((i) => i.id !== item.id)
  dest.items.push({ ...item })
}
</script>

<template>
  <div class="container">
    <nav>
      <ul>
        <li><a href="#" @click="createNewBoard">+ Create new list</a></li>
      </ul>
    </nav>
    <div class="boards-container">
      <div class="boards">
        <div
          class="board"
          @drop="onDrop($event, board)"
          @dragover.prevent
          @dragenter.prevent
          v-for="board in boards"
          :key="board.id"
        >
          <div class="board-name">{{ board.name }}</div>
          <div
            class="item drag-el"
            draggable="true"
            @dragstart="startDrag($event, board.id, item.id)"
            v-for="item in board.items"
            :key="item.id"
          >
            {{ item.title }}
          </div>
          <div class="input">
            <NewTask @on-new-item="(text) => handleNewItem(text, board)" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/base.scss';

.container {
  margin: 1rem 5rem;
  display: flex;
  flex-direction: column;
  gap: 20px;

  nav ul {
    padding: 0;
    margin: 10px 0;

    li {
      list-style: none;

      a {
        background-color: rgba(0, 0, 0, 0.33);
        padding: 10px;
        border-radius: 0.75rem;
        color: $primary-color;
      }
    }
  }

  .boards-container {
    .boards {
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      gap: 10px;

      .board {
        background: rgba(0, 0, 0, 0.7);
        padding: 10px;
        border-radius: 0.25rem;

        .board-name {
          margin: 0 10px 10px;
        }

        .drag-el {
          background-color: $secondary-color;
          color: $primary-color;
          margin-bottom: 10px;
          padding: 5px;
          border-radius: 0.25rem;
          cursor: pointer;
        }
      }
    }
  }
}

@media (width <= 600px) {
  .container {
    margin: 1rem 2rem;

    .boards-container .boards {
      flex-direction: column;
    }
  }
}
</style>
