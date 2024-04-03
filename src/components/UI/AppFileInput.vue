<template>
  <label class="file-input">
    <p class="file-input__label">{{ label }}</p>
    <div class="file-input__container">
      <input
        @change="handleFileChange"
        type="file"
        class="file-input__system-input"
        :accept="accept"
      />

      <button v-if="!file" @click="redirectClick" class="file-input__input">Выбрать файл</button>

      <button v-else @click.prevent.stop="deleteFile" class="file-input__input">Удалить</button>

      <AppLoader class="loader" v-if="loading" />

      <span class="file-input__changed-text">{{ file ? file.name : 'Файл не выбран' }}</span>
    </div>
    <p :class="['file-input__hint', error ? 'error' : '']">
      {{ error ? 'Error message' : hintText }}
    </p>
  </label>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import AppLoader from './AppLoader.vue'

const props = defineProps({
  accept: {
    type: String,
    default: ''
  },
  label: {
    type: String,
    default: 'Label'
  },
  hintText: {
    type: String,
    default: 'Hint text'
  }
})
const emit = defineEmits(['file-selected'])

const file = ref<File | null>(null)
const error = ref<true | false>(false)
const loading = ref<true | false>(false)

// функция обработки добавления файла с последующим эмитом в родителя
const handleFileChange = (event: Event) => {
  const target = event.target as HTMLInputElement
  const files = target.files
  const button = event.target.nextElementSibling

  event.target.setAttribute('disabled', 'true')
  button.setAttribute('disabled', 'true')
  loading.value = true
  setTimeout(() => {
    if (files && files.length > 0) {
      file.value = files[0]
      console.log(1, files)

      emit('file-selected', file)
    }
    loading.value = false
    event.target.removeAttribute('disabled')
    button.removeAttribute('disabled')
  }, 1000)
}

// функция редиректа клика с button на инпут
const redirectClick = (event: Event) => {
  // для списка инпутов можно добавить идентификаторы на кнопку и на инпут и сравнивая идентификаторы редиректить клик
  const input = event.target.previousElementSibling
  event.preventDefault()
  input.click()
}
const deleteFile = (event: Event) => {
  event.stopPropagation()
  console.log(event.target)
  const input = event.target.previousElementSibling
  // опять же логику описываю для одного инпута, для списка инпутов можно работать со вложенным объектом
  file.value = null
  input.files = file.value
}
</script>

<style lang="scss" scoped>
@import './input.scss';
@import '../../assets/base.scss';

.loader {
  width: 16px;
  height: 16px;
  margin-right: 4px;
}
</style>
