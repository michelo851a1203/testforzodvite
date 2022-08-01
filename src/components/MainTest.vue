<script setup lang="ts">
import { useField, useForm } from 'vee-validate';
import { toFormValidator } from '@vee-validate/zod';
import * as zod from 'zod';

const validationSchema = toFormValidator(zod.object({
  userName: zod.string({
    invalid_type_error: '請輸入文字',
    required_error: '請輸入文字',
  }).min(1, '請輸入使用者名稱'),
  userEmail: zod.string({
    invalid_type_error: '請輸入文字',
    required_error: '請輸入文字',
  }).email('請輸入信箱')
}));

const userName = useField('userName', undefined, {
  initialValue: '',
});

const userEmail = useField('userEmail', undefined, {
  initialValue: '',
});

const userAmount = useField('userAmount', undefined, {
  initialValue: 0,
});

const inputDate = useField('inputDate', undefined, {
  initialValue: '',
});

const userCurrentStatus = useField('userCurrentStatus', undefined, {
  initialValue: '',
});

const isEnabled = useField('isEnabled', undefined, {
  initialValue: false,
});

const { handleSubmit, reseForm } = useForm({
  validationSchema,
});

const currentSubmit = handleSubmit((submitInfo) => {
  console.log(submitInfo);
});

</script>

<template>
  <section>
    <div>

      <input
        class="rounded-xl px-3 py-2 border border-gray-400"
        type="text"
      >

    </div>
    <div>
      <button
        @click="currentSubmit"
        class="px-3 py-2 rounded-full bg-blue-300 hover:bg-blue-600"
      >

      </button>
    </div>
  </section>
</template>
