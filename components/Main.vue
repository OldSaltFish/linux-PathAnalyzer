<template>
  <div class="grid md:flex md:items-start">
    <!-- bg-[#eff0f3] -->
    <div class="relative">
      <textarea
        class="user-input bg-gray-400 outline outline-1 p-2 md:w-[360px] md:h-[80vh] w-full h-[270px]"
        v-model="userInput"
        name="path"
        id="p1"
      ></textarea>
      <UButton
        icon="icon-park:copy"
        class="absolute bottom-[8px] left-[8px]"
        @click="tryToCopy"
      >
        一键复制
      </UButton>
      <UButton icon="icon-park:analysis" class="absolute bottom-[8px] right-[8px]" @click="show">
        解析
      </UButton>
    </div>
    <div class="mx-8 flex-1 gap-4 grid auto-rows-min">
      <h2 class="text-2xl text-center">路径列表</h2>
      <div
        class="flex flex-1 justify-between items-center rounded-4 border-black border-2"
        v-for="(content, index) in pathList"
        :key="index"
      >
        <UTextarea v-model="pathList[index].value" variant="ghost" :rows=2 @change="handleItemChange" class="flex-1 bg-transparent" />
        <!-- <h4 class="ml-4">{{ content }}</h4> -->
        <UButton
          class="remove px-4 py-2 h-full"
          @click="removeItem(index)"
        >
          移除
        </UButton>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import ClipBoard from 'vue-clipboard3';
const { toClipboard } = ClipBoard();
const toast = useToast();
const userInput = ref('');
type PathItem = {
  value: string;
};
// const pathList = ref<string[]>([]);
const pathList = ref<PathItem[]>([]);
const show = () => {
  console.log(userInput.value);
  const pathSet = new Set(userInput.value.split(':'));
  const list = [...pathSet];
  pathList.value = [];
  list.forEach(item => {
    pathList.value.push({
      value: item
    });
  })

  // pathList.value = list;
  console.log(list);
};
const tryToCopy = async () => {
  try {
    await toClipboard(userInput.value);
    showSuccessToast();
  } catch (e) {
    showErrorToast();
  }
  function showSuccessToast() {
    // toast.add({
    //   title: '复制成功！',
    //   icon: 'icon-park:file-success',
    // });
    toast.add({
      title: '复制失败！',
      icon: 'icon-park:file-failed',
      color: 'error'
    });
  }
  function showErrorToast() {
    toast.add({
      title: '复制失败！',
      icon: 'icon-park:file-failed',
    });
  }
};
// item变化时的触发的函数
const handleItemChange = () => {
  console.log("Item-change");
  console.log(pathList.value);
  const newList = pathList.value.map(item => item.value)
  userInput.value = newList.join(':');
};

const removeItem = (index: number) => {
  pathList.value.splice(index, 1);
  const newList = pathList.value.map(item => item.value)
  userInput.value = newList.join(':');
};
</script>
<style scoped lang="scss"></style>
