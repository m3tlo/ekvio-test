<template>
  <form @submit.prevent="fetchForm">
    <AppFileInput
      hint-text="Загрузите изображение"
      accept="image/*"
      :input-error="error"
      error-message="Упс... что-то пошло не так"
      label="Аватар"
      @file-selected="handleFileSelected"
      @clean-status="cleanStatus"
    />
  </form>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import AppFileInput from './components/UI/AppFileInput.vue'

const error = ref<true | false>(false)

const getRandomInt = (max: number) => {
  return Math.floor(Math.random() * max)
}
const setError = (isStatusError: boolean) => {
  error.value = isStatusError
}

const handleFileSelected = (file: File) => {
  const random = getRandomInt(4)
  if (random === 2 || random === 0) {
    setError(true)
  } else setError(false)

  fetchForm(file)
}
const cleanStatus = () => {
  setError(false)
}

const fetchForm = (file: File) => {
  //   const response = await fetch("/api/form/post", {
  //   method: "post",
  //   headers: {
  //   'Accept': 'application/json',
  //    'Content-Type': 'application/json'
  //  },
  //  body: JSON.stringify({
  //      Сюда добавляем наш файл для отправки на сервер
  //   })

  console.log(`Отправка файла: ${file.name} на сервер...`)
}
</script>

<style lang="scss" scoped></style>
