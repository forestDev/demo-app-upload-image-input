<script setup lang="ts">
import { ref } from 'vue'

const preview = ref<HTMLImageElement>()
const showPreview = ref(false)

const previewPhoto = (event: Event) => {
  const input = event.target as HTMLInputElement
  if (input.files && input.files[0]) {
    const reader = new FileReader()
    reader.onload = function (e) {
      if (!preview.value || typeof e.target?.result !== 'string') {
        return
      }

      preview.value.src = e.target.result
      showPreview.value = true
    }
    reader.readAsDataURL(input.files[0])
  }
}
</script>

<template>
  <main class="main-container">
    <h1 class="title-header">Demo app</h1>
    <form class="photo-upload">
      <label for="photo" class="photo-label"> 📷 Zrób zdjęcie lub wybierz </label>
      <input
        type="file"
        id="photo"
        name="photo"
        accept="image/*"
        class="photo-input"
        @change="previewPhoto($event)"
      />
      <div class="preview-container" v-show="showPreview">
        <p>Podgląd zdjęcia:</p>
        <img ref="preview" class="preview" src="" alt="Podgląd zdjęcia" />
      </div>
    </form>
  </main>
</template>

<style scoped>
.preview-container {
  margin-top: 24px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.title-header {
  text-align: center;
  margin: 24px;
}

.photo-upload {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: sans-serif;
  margin: 20px;
}

.photo-label {
  background: linear-gradient(
    135deg,
    #f09433 0%,
    #e6683c 25%,
    #dc2743 50%,
    #cc2366 75%,
    #bc1888 100%
  );
  color: white;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: transform 0.2s ease;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  border: none;
}

.photo-label:hover {
  transform: scale(1.05);
}

.photo-input {
  display: none;
}

.preview {
  margin-top: 15px;
  max-width: 300px;
  height: auto;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}
</style>
