<script setup lang="ts">
import {ref} from "vue";

defineProps<{
  text?: string;
  height?: string;
  collapseable?: boolean;
  clickable?: boolean;
}>();

const emit = defineEmits<{
  (e: 'click');
}>();

const closed = ref(false);
</script>

<template>
  <div class="frame" :class="{clickable}" :style="{...(closed ? {height: '2rem'} : {height: (height || 8) + 'rem'}), ...(clickable ? {'user-select': 'none', 'cursor': 'pointer'} : {})}" @click="emit('click')">
    <i v-if="collapseable" @click="closed = !closed" class="fa-solid fa-chevron-down" style="position: absolute; right: 0.5rem; top: 0.5rem; cursor: pointer"></i>
    <span>{{text}}</span>

    <slot v-if="!closed"/>
  </div>
</template>

<style scoped lang="scss">
.frame {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  background: #fff;
  border: 1px solid #000;
  position: relative;
  * {
    text-align: center;
  }
  &.clickable:hover {
    color: #F3A847;
  }
}
</style>