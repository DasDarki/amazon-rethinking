<script setup lang="ts">
import Frame from "@/components/Frame.vue";
import {ref} from "vue";

const defaultAreas = ['Personalized Recommendations / Search History', 'Top Deals'];
const areas = ref<string[]>(localStorage.getItem('amazon_areas') ? JSON.parse(localStorage.getItem('amazon_areas')!) : [...defaultAreas]);

const newArea = ref('');

function addArea() {
  if (!newArea.value) return;
  areas.value.push('Custom Area: ' + newArea.value);
  newArea.value = '';
  saveAreas();
}

function bringAreaOneUp(area: string) {
  const index = areas.value.indexOf(area);
  if (index > 0) {
    const temp = areas.value[index - 1];
    areas.value[index - 1] = area;
    areas.value[index] = temp;
    saveAreas();
  }
}

function bringAreaOneDown(area: string) {
  const index = areas.value.indexOf(area);
  if (index < areas.value.length - 1) {
    const temp = areas.value[index + 1];
    areas.value[index + 1] = area;
    areas.value[index] = temp;
    saveAreas();
  }
}

function deleteArea(area: string) {
  if (defaultAreas.includes(area)) return;
  areas.value = areas.value.filter(a => a !== area);
  saveAreas();
}

function saveAreas() {
  localStorage.setItem('amazon_areas', JSON.stringify(areas.value));
}
</script>

<template>
  <div style="width: 100%; height: 100%; display: flex; justify-content: center; background: #E3E6E6">
    <div style="display: flex; flex-direction: column; width: 60%; padding-top: 1rem; gap: 1rem">
      <Frame v-for="(area, idx) in areas" :text="area" collapseable>
        <div style="display: flex; gap: 1rem; position: absolute; left: 0.5rem; top: 0.25rem">
          <button v-if="idx > 0" @click="bringAreaOneUp(area)">
            <i class="fa-solid fa-arrow-up"></i>
          </button>
          <button v-if="idx < areas.length - 1" @click="bringAreaOneDown(area)">
            <i class="fa-solid fa-arrow-down"></i>
          </button>
          <button v-if="!defaultAreas.includes(area)" @click="deleteArea(area)">
            <i class="fa-solid fa-trash"></i>
          </button>
        </div>
      </Frame>

      <Frame text="Add custom area" collapseable>
        <input v-model="newArea" @keyup.enter="addArea" placeholder="Add new area"/>
        <button @click="addArea">Add</button>
      </Frame>
    </div>
  </div>
</template>