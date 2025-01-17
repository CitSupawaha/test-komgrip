<script setup lang="ts">
import { RouterView, useRoute } from "vue-router";
import { sidebar } from "./data/Sidebar";
const route = useRoute();
</script>

<template>
  <div class="sidebar">
    <div class="logo">
      <img alt="Vue logo" src="@/assets/logo.png" width="60" height="60" />
      <p class="ml-2 text-xl">Komgrip Technologies</p>
    </div>
    <div class="-mt-4 border-b-2 mb-6"></div>
    <a
      v-for="r in sidebar"
      :key="r.name"
      :class="route.path == r.href ? 'active mb-4 shadow-md' : 'mb-4 '"
      :href="r.href"
    >
      <div class="flex items-center">
        <div
          v-html="r.svg"
          class="w-12 h-12 bg-[#c11c9f] rounded-xl shadow-md flex items-center justify-center"
        ></div>
        <p
          class="ml-4 text-xl"
          :class="route.path == r.href ? 'text-[#505f77]' : 'text-gray-500'"
        >
          {{ r.name }}
        </p>
      </div>
    </a>
  </div>
  <div class="content">
    <nav class="header" aria-label="Breadcrumb">
      <div class="flex justify-between items-center">
        <ol role="list" class="flex items-center space-x-2">
          <li class="text-xl">Pages</li>
          <li>
            <div class="flex items-center">
              <svg
                class="size-5 shrink-0 text-gray-300"
                fill="currentColor"
                viewBox="0 0 20 20"
                aria-hidden="true"
              >
                <path d="M5.555 17.776l8-16 .894.448-8 16-.894-.448z" />
              </svg>
              <a
                :href="route.path"
                class="ml-2 text-xl font-medium text-gray-500 hover:text-gray-700"
                :aria-current="route.path ? 'page' : undefined"
                >{{ route.name }}</a
              >
            </div>
          </li>
        </ol>
        <a class="flex items-center">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="gray"
            class="size-6"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M17.982 18.725A7.488 7.488 0 0 0 12 15.75a7.488 7.488 0 0 0-5.982 2.975m11.963 0a9 9 0 1 0-11.963 0m11.963 0A8.966 8.966 0 0 1 12 21a8.966 8.966 0 0 1-5.982-2.275M15 9.75a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"
            />
          </svg>

          <div class="flex items-center">
            <p class="ml-4 text-lg text-gray-500">Panuwit Hongsin</p>
          </div>
        </a>
      </div>
      <p class="mt-2 text-2xl font-bold">{{ route.name }}</p>
    </nav>

    <div class="mt-2"><RouterView /></div>
  </div>
</template>

<style lang="css">
.sidebar {
  margin: 0;
  padding: 2rem;
  width: 360px;
  position: fixed;
  height: 100%;
  overflow: auto;
}
.header {
  padding: 1.5rem;
}
.logo {
  display: flex;
  align-items: center;
  margin-bottom: 2rem;
  margin-left: 0.6rem;
}

.sidebar a {
  display: block;
  color: black;
  padding: 14px;
  text-decoration: none;
}

.sidebar a.active {
  background-color: white;
  box-shadow: #555;
  border-radius: 0.75rem;
}

.sidebar a:hover:not(.active) {
  background-color: white;
  box-shadow: #555;
  border-radius: 0.75rem;
  color: white;
}

div.content {
  margin-left: 360px;
  padding: 1px 16px;
  height: 1000px;
}

@media screen and (max-width: 700px) {
  .sidebar {
    width: 100%;
    height: auto;
    position: relative;
  }

  .sidebar a {
    float: left;
  }

  div.content {
    margin-left: 0;
  }
}

@media screen and (max-width: 400px) {
  .sidebar a {
    text-align: center;
    float: none;
  }
}
</style>
