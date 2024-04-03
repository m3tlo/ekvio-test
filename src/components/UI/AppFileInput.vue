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
    <p :class="['file-input__hint', inputError ? 'error' : '']">
      {{ inputError ? errorMessage : hintText }}
    </p>
  </label>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import AppLoader from './AppLoader.vue'

const props = defineProps({
  accept: {
    type: String,
    default: '',
    required: true
  },
  label: {
    type: String,
    default: 'Label',
    required: true
  },
  hintText: {
    type: String,
    default: 'Hint text',
    required: true
  },
  errorMessage: {
    type: String,
    default: 'Error message',
    required: true
  },
  inputError: {
    type: Boolean,
    default: false
  }
})
const emit = defineEmits(['file-selected', 'clean-status'])

const file = ref<File | null>(null)
const loading = ref<true | false>(false)

const setLoading = (isStatusLoading: boolean) => {
  loading.value = isStatusLoading
}
// функция обработки добавления файла с последующим эмитом в родителя
const handleFileChange = (event: Event) => {
  const target = event.target as HTMLInputElement
  const files = target.files
  const button = event.target.nextElementSibling

  event.target.setAttribute('disabled', 'true')
  button.setAttribute('disabled', 'true')
  setLoading(true)
  setTimeout(() => {
    if (files && files.length > 0) {
      file.value = files[0]

      emit('file-selected', file.value)
    }
    setLoading(false)
    event.target.removeAttribute('disabled')
    button.removeAttribute('disabled')
  }, 1000)
}

// функция редиректа клика с button на инпут
const redirectClick = (event: Event) => {
  // для списка инпутов можно добавить идентификаторы на кнопку и на инпут и сравнивая идентификаторы редиректить клик
  // так же обращение к элементам ниже можно изменить на обращение к атрибуту ref, но я решил оставить так
  const input = event.target.previousElementSibling
  event.preventDefault()
  input.click()
}
const deleteFile = (event: Event) => {
  event.stopPropagation()
  const input = event.target.previousElementSibling
  // опять же логику описываю для одного инпута, для списка инпутов можно работать со вложенным объектом
  file.value = null
  input.files = file.value

  emit('clean-status')
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
