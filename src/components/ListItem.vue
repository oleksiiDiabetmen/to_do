<script setup>
import { ref } from "vue";
const props = defineProps(["content", "itemStatus", "isPined"]);

const emit = defineEmits(["changeStatus", "pinItem", "removeItem"]);

const isVisible = ref(false);
const setVisible = () => {
  isVisible.value = !isVisible.value;
};
</script>

<template>
  <li class="list-item" @click.right="setVisible" :class="{ pinned: isPined }">
    <div class="item-content">{{ content }}</div>
    <div
      :class="{ done: itemStatus }"
      class="item-status"
      @click="$emit('changeStatus')"
    ></div>

    <Transition>
      <div class="remove-pin" v-if="isVisible">
        <button
          class="button-functionals pin"
          @click="$emit('pinItem')"
          v-if="!isPined"
        >
          pin
        </button>
        <button class="button-functionals pin" @click="$emit('pinItem')" v-else>
          unpin
        </button>
        <button class="button-functionals remove" @click="$emit('removeItem')">
          remove
        </button>
      </div>
    </Transition>
  </li>
</template>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: grid-template-rows 0.5s ease;
}

.v-enter-active .button-functionals,
.v-leave-active .button-functionals {
  transition: opacity 0.3s;
}

.v-enter-from,
.v-leave-to {
  grid-template-rows: 0px;
}

.v-enter-from .button-functionals,
.v-leave-to .button-functionals {
  opacity: 0;
}
</style>
