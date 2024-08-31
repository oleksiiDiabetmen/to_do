<script setup>
import ListItem from "./components/ListItem.vue";
import { ref, computed } from "vue";

const content = ref("");
const items = ref([]);
const index = ref(1);

const add = () => {
  if (content.value == "") {
    return;
  }

  items.value.push({
    i: index.value,
    content: content.value,
    done: false,
    isPined: false,
  });

  index.value++;
  content.value = "";
};

const needToDo = computed(() => {
  return items.value.filter((el) => el.done === false && el.isPined === false);
});

const done = computed(() => {
  return items.value.filter((el) => el.done === true && el.isPined === false);
});

const needToDoPined = computed(() => {
  return items.value.filter((el) => el.isPined === true && el.done === false);
});

const donePined = computed(() => {
  return items.value.filter((el) => el.isPined === true && el.done === true);
});

const changeStatus = (index) => {
  const item = items.value.find((e) => e.i == index);
  item.done = !item.done;
};

const pinItem = (index) => {
  const item = items.value.find((e) => e.i == index);
  item.isPined = !item.isPined;
};

const removeItem = (index) => {
  const idx = items.value.findIndex((e) => e.i == index);
  console.log(idx);
  items.value.splice(idx, 1);
};
</script>

<template>
  <header id="header">
    <input
      type="text"
      class="newElementContent"
      @keydown.enter="add"
      v-model="content"
    />
    <button class="add" @click="add">+</button>
  </header>
  <main id="main">
    <ul class="list to-do">
      <ListItem
        v-for="(item, index) in needToDoPined"
        :key="index"
        :content="item.content"
        :itemStatus="item.done"
        :isPined="item.isPined"
        @changeStatus="changeStatus(item.i)"
        @pinItem="pinItem(item.i)"
        @removeItem="removeItem(item.i)"
      />

      <ListItem
        v-for="(item, index) in needToDo"
        :key="index"
        :content="item.content"
        :itemStatus="item.done"
        :isPined="item.isPined"
        @changeStatus="changeStatus(item.i)"
        @pinItem="pinItem(item.i)"
        @removeItem="removeItem(item.i)"
      />
    </ul>

    <ul class="list done">
      <ListItem
        v-for="(item, index) in donePined"
        :key="index"
        :content="item.content"
        :itemStatus="item.done"
        :isPined="item.isPined"
        @changeStatus="changeStatus(item.i)"
        @pinItem="pinItem(item.i)"
        @removeItem="removeItem(item.i)"
      />

      <ListItem
        v-for="(item, index) in done"
        :key="index"
        :content="item.content"
        :itemStatus="item.done"
        :isPined="item.isPined"
        @changeStatus="changeStatus(item.i)"
        @pinItem="pinItem(item.i)"
        @removeItem="removeItem(item.i)"
      />
    </ul>
  </main>
</template>
