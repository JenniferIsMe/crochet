<template>
  <div class="knitting-joy-container p-4 bg-orange-50 min-h-screen">
    <header class="mb-6 text-center">
      <h1 class="text-2xl font-bold text-orange-800">织织乐 - 小熊毛衣</h1>
      <p class="text-sm text-gray-500">总进度: {{ totalProgress }}%</p>
    </header>

    <div class="relative overflow-hidden h-64 shadow-lg rounded-2xl bg-white">
      <div 
        class="flex transition-transform duration-300 ease-out h-full"
        :style="{ transform: `translateX(-${currentIndex * 100}%)` }"
        @touchstart="handleTouchStart"
        @touchend="handleTouchEnd"
      >
        <div 
          v-for="task in projectTasks" 
          :key="task.id" 
          class="min-w-full flex flex-col justify-center items-center p-8"
        >
          <span class="text-orange-400 font-medium mb-2">{{ task.name }}</span>
          <div class="text-6xl font-black text-gray-800 mb-4">
            {{ task.current }} <span class="text-xl text-gray-400">/ {{ task.target }}</span>
          </div>
          <div class="w-full bg-gray-100 h-2 rounded-full overflow-hidden">
            <div 
              class="bg-orange-400 h-full transition-all" 
              :style="{ width: (task.current / task.target * 100) + '%' }"
            ></div>
          </div>
        </div>
      </div>
    </div>

    <div class="mt-8 flex justify-between items-center px-10">
      <button @click="decrement" class="w-16 h-16 rounded-full bg-white shadow-md text-2xl">-</button>
      <button 
        @click="increment" 
        class="w-24 h-24 rounded-full bg-orange-500 shadow-xl text-white text-4xl font-bold active:scale-95 transition-transform"
      >
        +1
      </button>
      <button @click="reset" class="w-16 h-16 rounded-full bg-white shadow-md text-sm">重置</button>
    </div>

    <div class="flex justify-center mt-4 gap-2">
      <div 
        v-for="(_, index) in projectTasks" 
        :key="index"
        class="w-2 h-2 rounded-full transition-colors"
        :class="index === currentIndex ? 'bg-orange-500' : 'bg-gray-300'"
      ></div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

// 模拟从 Pinia 或后端获取的数据
const projectTasks = ref([
  { id: 1, name: '左袖子', current: 12, target: 40 },
  { id: 2, name: '右袖子', current: 5, target: 40 },
  { id: 3, name: '主体', current: 80, target: 120 }
]);

const currentIndex = ref(0);
let startX = 0;

// 手势处理：简单判断左右滑动
const handleTouchStart = (e) => { startX = e.touches[0].clientX; };
const handleTouchEnd = (e) => {
  const endX = e.changedTouches[0].clientX;
  const diff = startX - endX;
  if (Math.abs(diff) > 50) {
    if (diff > 0 && currentIndex.value < projectTasks.value.length - 1) {
      currentIndex.value++; // 左滑，下一张
    } else if (diff < 0 && currentIndex.value > 0) {
      currentIndex.value--; // 右滑，上一张
    }
  }
};

// 核心计数逻辑
const increment = () => {
  const task = projectTasks.value[currentIndex.value];
  if (task.current < task.target) task.current++;
  // 这里可以调用后端 PATCH 接口同步数据
};

const decrement = () => {
  const task = projectTasks.value[currentIndex.value];
  if (task.current > 0) task.current--;
};

const reset = () => {
  if (confirm('确定要重置当前任务吗？')) {
    projectTasks.value[currentIndex.value].current = 0;
  }
};

const totalProgress = computed(() => {
  const total = projectTasks.value.reduce((acc, t) => acc + t.target, 0);
  const current = projectTasks.value.reduce((acc, t) => acc + t.current, 0);
  return Math.round((current / total) * 100);
});
</script>