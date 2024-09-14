<script lang="ts" setup>
import { defineProps, defineEmits, computed } from "vue";
import { IThings } from "./products.data";

const emit = defineEmits<{
  (e: "select", id: number): void;
}>();
const props = defineProps<{
  name: string;
  id: number;
  selectedCards?: [];
}>();

function select() {
  emit("select", props.id);
}
const isActive = computed(() => {
  if (!props.selectedCards) return false;
  return props.selectedCards.find((card: IThings) => card.id === props.id);
});
</script>
<template>
  <div @click="select" :class="{ active: isActive }" class="card-select">
    <span class="card-select__text">{{ name }}</span>
  </div>
</template>
<style lang="scss" scoped>
.card-select {
  cursor: pointer;
  padding: 8px;
  border: 5px solid #000;

  &__text {
    font-size: 18px;
    line-height: 24px;
  }

  &.active {
    background: #000;
    color: #fff;
  }
}
</style>
