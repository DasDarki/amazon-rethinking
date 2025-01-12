<script setup lang="ts">
import {useRoute} from "vue-router";
import {computed} from "vue";
import Frame from "@/components/Frame.vue";
import router from "@/router";

const route = useRoute();
const searchText = computed(() => route.query.s as string);
</script>

<template>
  <div style="width: 100%; padding: 0.75rem; background: white; box-shadow: 0 0 0.5rem rgba(0, 0, 0, 0.6); display: flex; justify-content: center; align-content: center">
    <div style="width: 65%; display: flex; align-content: center; justify-content: space-between; font-size: 0.9rem">
      <span>1-20 of more than X.000 results or suggestions for <span style="color: #F3A847; font-weight: bold">"{{searchText}}"</span></span>

      <select>
        <option>Sort by: Relevance</option>
        <option>Sort by: Price</option>
        <option>Sort by: Newest</option>
        <option>Sort by: Rating</option>
        <option>Sort by: Suggestions</option>
        <option>Sort by: Manufacturer</option>
      </select>
    </div>
  </div>

  <div style="width: 100%; display: flex; justify-content: center; margin-top: 1rem">
    <div style="display: flex; flex-direction: row; width: 65%; padding-top: 1rem; padding-bottom: 1rem; gap: 2rem">
      <Frame text="Filter Options" height="30" style="width: 13rem"/>

      <div style="width: calc(90% - 15rem); display: flex; flex-direction: column; gap: 1rem">
        <h2 style="margin-top: -0.5rem; margin-bottom: 0">Results</h2>

        <Frame v-for="i in 20" :key="i" :text="`Product ${i}: ${searchText}`" height="8" clickable @click="router.push({name: 'product', query: {p: `Product ${i}: ${searchText}`}})"/>
      </div>
    </div>
  </div>

  <div style="position: fixed; right: 5rem; top: 14rem">
    <Frame :text="`Some other not related ${searchText}`" style="width: 20rem" height="20" collapseable>
      <div style="width: 100%; height: 2rem; color: #fff; background: #F3A847; position: absolute; top: 0; left: 0; display: flex; justify-content: center; align-content: center; flex-direction: column; font-weight: bold; border-bottom: 1px solid #000">
        Sponsored
      </div>
    </Frame>
  </div>
</template>