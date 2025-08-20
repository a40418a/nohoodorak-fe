<template>
  <div>
    <div class="mb-2 flex items-end justify-between">
      <span class="text-primary-500 text-lg font-semibold">{{ title }}</span>
      <span v-if="explain" class="text-surface-300">{{ explain }}</span>
    </div>

    <!-- 날짜 타입: v-calendar 사용 -->
    <template v-if="type === 'date'">
      <DatePicker v-model="dateValue" :masks="{ modelValue: 'YYYY-MM-DD' }" hide-time-header>
        <template #default="{ togglePopover, inputValue }">
          <InputBox
            :modelValue="displayFormatter ? displayFormatter(inputValue) : inputValue"
            :placeholder="placeholder"
            size="large"
            type="text"
            readonly
            @click="togglePopover"
            class="cursor-pointer"
          />
        </template>
      </DatePicker>
    </template>

    <!-- 나머지 타입 -->
    <template v-else>
      <InputBox
        :modelValue="modelValue"
        @update:modelValue="$emit('update:modelValue', $event)"
        :placeholder="placeholder"
        size="large"
        :type="type"
        :readonly="readOnly"
      />
    </template>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import InputBox from '@/components/forms/InputBox.vue';
import { DatePicker } from 'v-calendar';
import 'v-calendar/style.css';

const props = defineProps<{
  title: string;
  explain?: string;
  modelValue: string; // YYYY-MM-DD 형식
  type?: 'text' | 'password' | 'number' | 'date' | 'time';
  placeholder?: string;
  readOnly?: boolean;
  displayFormatter?: (v: string) => string;
}>();

const emit = defineEmits<{
  (e: 'update:modelValue', v: string): void;
}>();

// v-calendar가 Date 객체를 사용하므로, YYYY-MM-DD 문자열과 변환
const dateValue = computed({
  get() {
    // modelValue가 유효한 YYYY-MM-DD 형식일 때만 Date 객체로 변환
    return props.modelValue && props.modelValue.match(/^\d{4}-\d{2}-\d{2}$/)
      ? new Date(props.modelValue)
      : null; // 형식이 아니면 null
  },
  set(val) {
    if (val instanceof Date) {
      const year = val.getFullYear();
      const month = (val.getMonth() + 1).toString().padStart(2, '0');
      const day = val.getDate().toString().padStart(2, '0');
      emit('update:modelValue', `${year}-${month}-${day}`);
    }
  },
});
</script>