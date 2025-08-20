<template>
  <div
    class="btn-pressed bg-secondary-100 flex flex-col gap-4 rounded-2xl p-6 shadow-lg"
    @click="onClick"
  >
    <!-- 아이콘 영역 -->
    <img :src="src" class="h-auto w-full" alt="이벤트 이미지" />

    <!-- 텍스트 -->
    <div class="flex flex-col">
      <div class="text-primary-500 mb-2 text-2xl font-bold">{{ title }}</div>
      <div>{{ content1 }}</div>
      <div>{{ content2 }}</div>
    </div>
  </div>
</template>
<script setup lang="ts">
defineOptions({ inheritAttrs: false });
defineProps<{
  title?: string;
  content1?: string;
  content2?: string;
  color?: 'primary' | 'secondary';
  src?: string;
}>();

const emit = defineEmits<{
  (e: 'click'): void;
}>();

function onClick() {
  emit('click');
}
</script>
