<script setup lang="ts">
import axios from "axios";
import { onMounted, ref } from "vue";
const coins = ref<any>([]);
const data = ref<any>([]);
const offset = ref(0);
const fetchData = async (limit = 5, offset = 0) => {
  try {
    const response = await axios.get(
      `https://komgrip.co.th/coincap/assets?limit=${limit}&offset=${offset}`
    );
    coins.value = response.data.data;
    if (offset == 0) {
      data.value = response.data.data;
    }
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};

onMounted(() => fetchData());

const pareString = (value: string) =>
  parseFloat(value || "0").toFixed(2) || "NO LIMIT";

const textPercent = (value: string) =>
  parseFloat(value) > 0 ? "text-green-500" : "text-red-500";

const goBack = () => {
  offset.value = Math.max(offset.value - 5, 0);
  fetchData(5, offset.value);
};

const nextPage = () => {
  offset.value += 5;
  fetchData(5, offset.value);
};
</script>

<template>
  <main>
    <div class="grid grid-cols-4 gap-x-8">
      <div
        class="bg-white rounded-3xl shadow-md p-6"
        v-for="i in data.slice(0, 4)"
        :key="i"
      >
        <div class="flex justify-between items-center">
          <div>
            <h2 class="text-xl text-gray-500">{{ i.name }}</h2>
            <p class="text-gray-600 font-bold text-2xl mt-2">
              ${{ pareString(i.priceUsd) }}
            </p>
          </div>
          <div
            class="bg-gradient-to-tl from-pink-500 to-purple-500 rounded-xl flex items-center justify-center h-16 w-16 shadow-md"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="white"
              class="size-6"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M20.25 6.375c0 2.278-3.694 4.125-8.25 4.125S3.75 8.653 3.75 6.375m16.5 0c0-2.278-3.694-4.125-8.25-4.125S3.75 4.097 3.75 6.375m16.5 0v11.25c0 2.278-3.694 4.125-8.25 4.125s-8.25-1.847-8.25-4.125V6.375m16.5 0v3.75m-16.5-3.75v3.75m16.5 0v3.75C20.25 16.153 16.556 18 12 18s-8.25-1.847-8.25-4.125v-3.75m16.5 0c0 2.278-3.694 4.125-8.25 4.125s-8.25-1.847-8.25-4.125"
              />
            </svg>
          </div>
        </div>
        <p
          class="text-[#8bd527] mt-2 text-xl font-semibold"
          :class="textPercent(i.changePercent24Hr)"
        >
          {{ pareString(i.changePercent24Hr) }}%
        </p>
      </div>
    </div>
    <div class="bg-white rounded-3xl shadow-md mt-8">
      <div class="flex justify-between items-center mb-4 p-8">
        <h2 class="text-2xl font-semibold">Cryptocurrencies</h2>
        <div class="flex space-x-4">
          <button
            class="bg-white border border-[#c11c9f] hover:bg-gray-300 text-gray-700 font-bold py-2 px-8 rounded-lg"
            @click="goBack()"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5"
              fill="none"
              viewBox="0 0 24 24"
              stroke="#c11c9f"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M15 19l-7-7 7-7"
              />
            </svg>
          </button>
          <button
            class="bg-white border border-[#c11c9f] hover:bg-gray-300 text-gray-700 font-bold py-2 px-8 rounded-lg"
            @click="nextPage()"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5"
              fill="none"
              viewBox="0 0 24 24"
              stroke="#c11c9f"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M9 5l7 7-7 7"
              />
            </svg>
          </button>
        </div>
      </div>

      <table
        class="w-full table-auto border-collapse border-b border-gray-200 text-gray-400 text-xl"
      >
        <thead>
          <tr>
            <th class="border-b border-gray-200 p-4 text-center">NO.</th>
            <th class="border-b border-gray-200 p-4 text-center">NAME</th>
            <th class="border-b border-gray-200 p-4 text-center">SYMBOL</th>
            <th class="border-b border-gray-200 p-4 text-right">
              SUPPLY / MAX SUPPLY
            </th>
            <th class="border-b border-gray-200 p-4 text-right">USD</th>
            <th class="border-b border-gray-200 p-4 text-center">24 HR</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in coins" :key="item" class="font-normal">
            <td class="border-b border-gray-200 p-4 text-center">
              {{ item?.rank }}
            </td>
            <td class="border-b border-gray-200 p-4 text-center">
              {{ item.name }}
            </td>
            <td class="border-b border-gray-200 p-4 text-center">
              {{ item.symbol }}
            </td>
            <td class="border-b border-gray-200 p-4 text-right">
              {{ pareString(item.supply) }} / {{ pareString(item.maxSupply) }}
            </td>
            <td class="border-b border-gray-200 p-4 text-right">
              ${{ pareString(item?.priceUsd) }}
            </td>
            <td
              class="border-b border-gray-200 p-4 text-green-500 text-center"
              :class="textPercent(item.changePercent24Hr)"
            >
              {{ pareString(item.changePercent24Hr) }}%
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </main>
</template>
<style scoped></style>
