<template>
  <div :class="$style.wrapper">
    <div
      v-for="category in categories"
      :key="category.id"
      @drop="onDrop($event, category.id)"
      :class="$style.droppable"
      @dragover.prevent
      @dragenter.prevent
    >
      <h2>{{ category.title }}</h2>
      <div
        v-for="item in items.filter((i) => i.categoryId === category.id)"
        :key="item.id"
        @dragstart="onDragStart($event, item)"
        :class="$style.draggable"
        draggable="true"
      >
        <h5>{{ item.title }}</h5>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    let items = ref([
      {
        id: 0,
        title: "Audi",
        categoryId: 0,
      },
      {
        id: 1,
        title: "Mers",
        categoryId: 0,
      },
      {
        id: 2,
        title: "Cat",
        categoryId: 1,
      },
    ]);
    const categories = ref([
      {
        id: 0,
        title: "Cars",
      },
      {
        id: 1,
        title: "Animals",
      },
    ]);
    function onDragStart(e, item) {
      e.dataTransfer.dropEffect = "move";
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.setData("itemId", item.id.toString());
    }
    function onDrop(e, categoryId) {
      const itemId = parseInt(e.dataTransfer.getData("itemId"));
      items.value = items.value.map((x) => {
        if (x.id === itemId) {
          x.categoryId = categoryId;
          return x;
        }
      });
    }
    return {
      items,
      categories,
      onDrop,
      onDragStart,
    };
  },
};
</script>

<style lang="scss" module>
.wrapper {
  .droppable {
    padding: 10px;
    border: 1px solid black;
    max-width: 300px;
    background-color: rgb(45, 70, 70);
    color: white;
    .draggable {
      padding: 10px;
      border: 1px solid black;
      max-width: 100%;
      background-color: white;
      color: black;
      text-align: center;
      margin: 0 0 5px 0;
    }
  }
}
</style>
