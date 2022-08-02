<script setup lang="ts">
import { computed } from 'vue';
import { InputCategoryType } from '../datatype/mainTest';

const props = withDefaults(defineProps<{
  title: string;
  currentValue: number;
  errorMessage?: string;
  inputCluster: InputCategoryType[];
}>(), {
  title: '',
  currentValue: -1,
});

const generateRandomString = () => Math.random().toString(16).slice(2);

const inputClusterReference = computed(() => props.inputCluster.map((inputItem) => ({
  id: `${generateRandomString()}_${inputItem.inputText}`,
  inputValue: inputItem.inputValue,
  inputText: inputItem.inputText,
})));

const emit = defineEmits<{(e: 'update:currentValue', currentInput: number): void,
}>();

const checkBoxChangeEventHandler = (checkboxEvent: Event, checkboxValue: number) => {
  const checkboxElement = <HTMLInputElement>checkboxEvent.target;
  if (checkboxElement.checked) {
    emit('update:currentValue', checkboxValue);
    return;
  }
  emit('update:currentValue', -1);
};

</script>

<template>
  <div
    class="flex items-center space-x-4 mt-4"
  >
    <div>
      <span class="text-red-500">*</span>
      {{ title }}
    </div>
    <div
      v-for="inputItem in inputClusterReference"
      :key="inputItem.id"
      class="flex items-center space-x-2"
    >
      <input
        @change="checkBoxChangeEventHandler($event, inputItem.inputValue)"
        :checked="inputItem.inputValue === currentValue"
        :id="inputItem.id"
        type="checkbox"
      >
      <label :for="inputItem.id">
        {{ inputItem.inputText }}
      </label>
    </div>
  </div>
  <div
    v-if="errorMessage"
    class="text-red-500"
  >
    <small>
      {{ errorMessage }}
    </small>
  </div>
</template>
