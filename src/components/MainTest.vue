<script setup lang="ts">
import { useField, useForm } from 'vee-validate';
import { toFormValidator } from '@vee-validate/zod';
import * as zod from 'zod';
import { CurrentUserStatus, InputCategoryType } from '../datatype/mainTest';
import BaseInput from './BaseInput.vue';
import BaseNumberInput from './BaseNumberInput.vue';
import BaseCheckBoxGroup from './BaseCheckBoxGroup.vue';
import { Ref, ref } from 'vue';


const validationSchema = toFormValidator(zod.object({
  userName: zod.string({
    invalid_type_error: '請輸入文字',
    required_error: '請輸入文字',
  }).min(1, '請輸入使用者名稱'),
  userEmail: zod.string({
    invalid_type_error: '請輸入文字',
    required_error: '請輸入文字',
  }).email('請輸入信箱').min(1, '請輸入信箱'),
  userAmount: zod.number({
    invalid_type_error: '請輸入數字',
    required_error: '此欄位必須有',
  }),
  inputDate: zod.string().refine((inputDateInfo) => {
    const inputTestDate = Date.parse(inputDateInfo);
    if (Number.isNaN(inputTestDate)) return false;
    return true;
  }, {
    message: '請輸入日期格式',
  }),
  currentUserStatus: zod.nativeEnum(CurrentUserStatus),
  isEnabled: zod.boolean({
    required_error: '需要需要狀態',
    invalid_type_error: '無效的狀態',
  }),
}));

const inputCategory: Ref<InputCategoryType[]> = ref([
  {
    inputValue: CurrentUserStatus.IS_GOOD,
    inputText: '好',
  },
  {
    inputValue: CurrentUserStatus.IS_OKAY,
    inputText: '還行',
  },
  {
    inputValue: CurrentUserStatus.IS_BAD,
    inputText: '不好',
  },
])

const {
  value: userName,
  errorMessage: userNameError,
} = useField('userName', undefined, {
  initialValue: '',
});

const {
  value: userEmail,
  errorMessage: userEmailError,
} = useField('userEmail', undefined, {
  initialValue: '',
});

const {
  value: userAmount,
  errorMessage: userAmountError,
} = useField('userAmount', undefined, {
  initialValue: 0,
});

const {
  value: inputDate,
  errorMessage: inputDateError,
} = useField('inputDate', undefined, {
  initialValue: '',
});

const {
  value: currentUserStatus,
  errorMessage: currentUserStatusError,
} = useField('currentUserStatus', undefined, {
  initialValue: CurrentUserStatus.IS_OKAY,
});

const {
  value: isEnabled,
  errorMessage: isEnabledError,
} = useField('isEnabled', undefined, {
  initialValue: false,
});

const { handleSubmit, resetForm } = useForm({
  validationSchema,
});

const currentSubmit = handleSubmit((submitInfo) => {
  console.log(submitInfo);
});

const resetAllPlatform = () => {
  resetForm();
};

</script>

<template>
  <section>
    <BaseInput
      title="使用者名稱"
      v-model:inputValue="userName"
      :errorMessage="userNameError"
    ></BaseInput>

    <BaseInput
      title="使用者信箱"
      v-model:inputValue="userEmail"
      :errorMessage="userEmailError"
      textInputType="email"
    ></BaseInput>

    <BaseNumberInput
      title="使用者輸入數字"
      v-model:inputValue="userAmount"
      :errorMessage="userAmountError"
    >
    </BaseNumberInput>

    <BaseInput
      title="輸入日期"
      v-model:inputValue="inputDate"
      :errorMessage="inputDateError"
      textInputType="date"
    >
    </BaseInput>

    <BaseCheckBoxGroup
      title="分類:"
      :inputCluster="inputCategory"
      v-model:currentValue="currentUserStatus"
      :errorMessage="currentUserStatusError"
    >
    </BaseCheckBoxGroup>

    <div class="flex items-center space-x-4 mb-4">
      <label for="current_checkbox">
        <span class="text-red-500">*</span>
        是否啟用
      </label>
      <input 
        v-model="isEnabled"
        id="current_checkbox" 
        type="checkbox"
      >
    </div>
    <div v-if="isEnabledError">
      <small class="text-red-500">
         {{ isEnabledError }}
      </small>
    </div>

    <div 
      class="space-x-4"
    >
      <button
        @click="resetAllPlatform"
        class="px-4 py-3 rounded-full bg-blue-300 border-gray-100
        hover:bg-blue-600 hover:text-white border"
      >
        重設
      </button>
      <button
        @click="currentSubmit"
        class="px-4 py-3 rounded-full bg-blue-300 border-gray-100
        hover:bg-blue-600 hover:text-white border"
      >
        點擊測試送出
      </button>
    </div>
  </section>
</template>
