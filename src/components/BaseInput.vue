<script setup lang="ts">
import { computed } from 'vue';

const props = withDefaults(defineProps<{
  title: string;
  inputValue: string;
  errorMessage?: string;
  textInputType?: string;
}>(), {
  inputValue: '',
  title: '',
  textInputType: 'text',
});

const inputRefId = computed(() => {
  const randomString = Math.random().toString(16).slice(2);
  return `${randomString}_${props.title}`;
});

const emits = defineEmits<{(e: 'update:inputValue', currentInput: string): void,
}>();

const inputTextboxHandler = (inputEvent: Event) => {
  const inputElement = <HTMLInputElement>inputEvent.target;
  emits('update:inputValue', inputElement.value);
};

</script>

<template>
    <div
      class="flex items-center space-x-4 mt-4"
    >
      <label
        :for="inputRefId"
      >
        <span class="text-red-500">*</span>
        {{ title }}
      </label>
      <input
        @input="inputTextboxHandler"
        :value="inputValue"
        :id="inputRefId"
        class="rounded-xl px-3 py-2 border border-gray-400"
        :type="textInputType"
      >
    </div>
    <div
      v-if="errorMessage"
      class="text-red-500"
    >
      <small>{{ errorMessage }}</small>
    </div>
</template>
