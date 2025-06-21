<script setup>
  import { ref } from 'vue';

  const items = ref([
    { id: '1', text: 'first item', backgroundColor: 'lightblue' },
    { id: '2', text: 'second item', backgroundColor: 'pink' },
    { id: '3', text: 'third item', backgroundColor: 'yellow' },
    { id: '4', text: 'fourth item', backgroundColor: 'lightgreen' },
    { id: '5', text: 'fifth item', backgroundColor: 'violet' },
  ]);

  const dragStartIndex = ref(null)
  function dragStart(index) {
    dragStartIndex.value = index;
  }

  function onDrop(event) {
    const draggedEl = event.currentTarget;
    const children = [...draggedEl.parentElement.children];
    const dragTargetIndex = children.indexOf(draggedEl);
    if (dragStartIndex.value === dragTargetIndex) {
      return;
    }
    const updated = [...items.value];
    const [movedItem] = updated.splice(dragStartIndex.value, 1);
    updated.splice(dragTargetIndex, 0, movedItem);
    items.value = updated;
    dragStartIndex.value = null;
  }
</script>

<template>
  <main>
    <div
      v-for="(item, index) in items"
      :key="item.id"
      class="item-container"
      :style="{ backgroundColor: item.backgroundColor }"
      draggable="true"
      @dragstart="dragStart(index)"
      @dragover.prevent
      @drop="onDrop"
    >
      {{ item.text }}
    </div>
  </main>
</template>

<style scoped>
  .item-container {
    height: 300px;
    width: 300px;
    text-align: center;
    margin-bottom: 10px;
    font-size: larger;
  }
</style>
