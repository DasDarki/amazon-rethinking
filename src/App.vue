<script setup lang="ts">
import { RouterView } from 'vue-router'
import router from "@/router";
import {ref} from "vue";

const searchText = ref('');
const showPrivacyDropdown = ref(false);
</script>

<template>
  <div class="page-wrapper">
    <nav>
      <h1 style="user-select: none; cursor: pointer;" @click="searchText = ''; router.push({name: 'home'})">amazon.de</h1>

      <div style="display: flex">
        <i class="fa-solid fa-map-marker" style="margin-right: 0.5rem; margin-top: 0.5rem"></i>
        <div style="display: flex; flex-direction: column; font-size: 0.9rem">
          <span>Deliver to</span>
          <b>Germany</b>
        </div>
      </div>

      <div class="search-group">
        <select>
          <option value="All">All</option>
        </select>

        <input v-model="searchText"/>

        <button @click="router.push({name: 'search', query: {s: searchText}})">
          <i class="fa-solid fa-search"></i>
        </button>
      </div>

      <div style="margin-left: auto; position: relative; user-select: none; cursor: pointer">
        <span @click="showPrivacyDropdown = !showPrivacyDropdown">Privacy Settings</span>

        <div v-if="showPrivacyDropdown" class="privacy-dropdown" @click="showPrivacyDropdown = false" @mouseleave="showPrivacyDropdown = false">
          <span>Privacy Policy</span>
          <span>View User Data</span>
          <span>Download User Data</span>
          <span style="color: red">Delete User Data</span>
        </div>
      </div>
    </nav>

    <main>
      <RouterView />
    </main>

    <footer>
      Footer
    </footer>
  </div>

</template>

<style scoped lang="scss">
@use "main";

.page-wrapper {
  font-family: sans-serif;
  position: absolute;
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;

  nav {
    width: calc(100% - 3rem);
    padding: 0.25rem 1.5rem;
    color: #fff;
    background: main.$color_nav;
    border-bottom: 2rem solid main.$color_nav_secondary;
    display: flex;
    align-items: center;
    gap: 2rem;

    .search-group {
      width: 65%;
      display: flex;
      border: 2px solid transparent;
      border-radius: 0.3rem;
      select {
        height: 2rem;
        border: none;
        border-right: 1px solid #D4D4D4;
        padding: 0 0.5rem;
        color: #777774;
        background: #E6E6E6;
        border-radius: 0.25rem 0 0 0.25rem;
        &:hover {
          cursor: pointer;
          background: #D4D4D4;
        }
      }
      input {
        width: 100%;
        height: 2rem;
        border: none;
        padding: 0 0.5rem;
        color: #000;
        outline: none;
        &:focus {
          outline: none;
        }
      }
      button {
        height: 2rem;
        border: none;
        padding: 0 1rem;
        background: main.$color_primary;
        border-radius: 0 0.25rem 0.25rem 0;
        &:hover {
          cursor: pointer;
          background: main.$color_primary_hover;
        }
      }
      &:has(> input:focus) {
        border: 2px solid main.$color_primary;
      }
    }

    .privacy-dropdown {
      position: absolute; top: 2rem; right: 0; background: #131921; border: 1px solid #000; border-radius: 0.5rem; padding: 0.5rem 1rem; display: flex; flex-direction: column; gap: 0.5rem; width: 10rem; text-align: right; box-shadow: 0 0 0.5rem rgba(0, 0, 0, 0.5);
      span {
        color: #fff;

        &:hover {
          cursor: pointer;
          text-decoration: underline;
        }
      }
    }
  }

  main {
    width: 100%;
    flex: 1;
    overflow-x: hidden;
    overflow-y: auto;
  }

  footer {
    width: calc(100% - 2rem);
    padding: 1rem;
    color: #fff;
    background: #232F3E;
    text-align: center;
    font-size: 1.5rem;
  }
}
</style>
