<template>
  <div class="flex">
    <!-- bg-[#eff0f3] -->
    <div class="relative">
      <textarea
      class="user-input bg-transparent outline outline-1 p-2 w-[360px] h-[80vh]"
      v-model="userInput"
      name="path"
      id="p1"
    ></textarea>
    <button class="absolute bottom-[8px] right-[8px]" @click="show">解析</button>
    </div>
    <div class="mx-8 flex-1 gap-4 grid auto-rows-min">
      <h2 class="text-2xl text-center">路径列表</h2>
      <div class="flex flex-1 justify-between items-center rounded-4 border-black border-2" v-for="(content, index) in pathList" :key="index">
        <h4 class="ml-4">{{ content }}</h4>
        <button class="remove px-4 py-2 outline outline-1" @click="removeItem(index)">移除</button>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
const userInput = ref('');
const pathList = ref<string[]>([]);
const show = () => {
  console.log(userInput.value);
  const pathSet = new Set(userInput.value.split(':'));
  const list = [...pathSet];
  pathList.value = list;
  console.log(list);
};
const removeItem = (index: number) => {
  pathList.value.splice(index, 1);
  userInput.value = pathList.value.join(':');
};
</script>
<style scoped lang="scss"></style>
