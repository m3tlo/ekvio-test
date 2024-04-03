<template>
  <AppFileInput
    hint-text="Загрузите изображение"
    accept="image/*"
    @file-selected="handleFileSelected"
  />
</template>

<script setup lang="ts">
import AppFileInput from './components/UI/AppFileInput.vue'

const handleFileSelected = (file: File) => {
  console.log(`Отправка файла: ${file} на сервер...`)
}
</script>

<style lang="scss" scoped></style>
