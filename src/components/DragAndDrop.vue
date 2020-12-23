<template>
  <div class="drag-container" v-drag-and-drop:options="options">
    <ul class="drag-list">
      <li class="drag-column" v-for="group in groups" :key="group.id">
        <span class="drag-column-header">
          <h2>{{ group.name }}</h2>
          <feather-icon type="more-vertical"></feather-icon>
        </span>

        <vue-draggable-group v-model="group.items" :groups="groups" :data-id="group.id" @change="onGroupsChange">
          <ul class="drag-inner-list" :data-id="group.id">
            <li class="drag-item" v-for="item in group.items" :key="item.id" :data-id="item.id">
              <div class="drag-item-text">{{ item.name }}</div>
            </li>
            <div v-if="group.id == 1">
              <input type="text" placeholder="Добавит карточку" @keyup.enter="addCard" v-model="newTodoName" />
            </div>
          </ul>
        </vue-draggable-group>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodoName: "",
      groups: [
        {
          id: 1,
          name: "Список задач",
          items: [{ id: 1, name: "Принимат звонки", groupId: 1 }],
        },
        {
          id: 2,
          name: "В процессе",
          items: [
            { id: 2, name: "Встреча с клиентом", groupId: 2 },
            { id: 3, name: "Запланироват вебинар", groupId: 2 },
            { id: 4, name: "Рассылка по электронной почте", groupId: 2 },
          ],
        },
        {
          id: 3,
          name: "Выполнено",
          items: [
            { id: 5, name: "Опубликуйте подкаст", groupId: 3 },
            { id: 6, name: "Запустит сайт", groupId: 3 },
          ],
        },
      ],
      options: {
        dropzoneSelector: ".drag-inner-list",
        draggableSelector: ".drag-item",
      },
    };
  },
  methods: {
    onGroupsChange(e) {
      console.log({ e });
    },
    addCard() {
      this.groups[0].items.push({ id: this.groups.reduce((acc, cur) => acc + cur.items.length, 0) + 1, name: this.newTodoName, groupId: 1 });
      this.newTodoName = "";
    },
  },
};
</script>

<style lang="scss">
$ease-out: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
$to-do: #f4ce46;
$in-progress: #2a92bf;
$approved: #00b961;

* {
  box-sizing: border-box;
}

body {
  background: linear-gradient(0deg, #eecfba, #c5dde8);
  color: white;
  font-family: "Roboto Mono", serif;
  font-weight: 300;
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.drag-container {
  max-width: 1000px;
  margin: 20px auto;
}

.drag-list {
  display: flex;
  align-items: flex-start;

  @media (max-width: 690px) {
    display: block;
  }
}

.drag-column {
  flex: 1;
  margin: 0 10px;
  position: relative;
  background: rgba(rgb(255, 255, 255), 0.2);
  overflow: hidden;
  border-radius: 10px;

  @media (max-width: 690px) {
    margin-bottom: 30px;
  }

  h2 {
    font-size: 0.8rem;
    margin: 0;
    text-transform: uppercase;
    font-weight: 600;
  }

  &-to-do {
    .drag-column-header,
    .drag-options {
      background: $to-do;
    }
  }

  &-in-progress {
    .drag-column-header,
    .drag-options {
      background: $in-progress;
    }
  }

  &-approved {
    .drag-column-header,
    .drag-options {
      background: $approved;
    }
  }
}

.drag-column-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  user-select: none;
}

.drag-inner-list {
  height: 85vh;
  overflow: auto;
}

.drag-item {
  margin: 10px;
  height: 100px;
  background: rgba(rgb(255, 255, 255), 0.8);
  transition: $ease-out;
  border-radius: 10px;

  /* items grabbed state */
  &[aria-grabbed="true"] {
    background: #5cc1a6;
    color: #fff;
  }

  .drag-item-text {
    font-size: 1rem;
    padding-left: 1rem;
    padding-top: 1rem;
  }
}

.drag-header-more {
  cursor: pointer;
}

@keyframes nodeInserted {
  from {
    opacity: 0.2;
  }
  to {
    opacity: 0.8;
  }
}

.item-dropzone-area {
  height: 6rem;
  background: #888;
  opacity: 0.8;
  animation-duration: 0.5s;
  animation-name: nodeInserted;
  margin-left: 0.6rem;
  margin-right: 0.6rem;
}

input {
  font-size: 16px;
  border: none;
  width: 90%;
  outline: 0;
  font-family: "Helvetica Neue", Helvetica, Tahoma, Arial, sans-serif;
  padding: 10px 14px;
  border-radius: 7px;
  color: rgb(39, 39, 39);
  background: rgba(255, 255, 255, 0.2);
  cursor: pointer;
}
</style>
