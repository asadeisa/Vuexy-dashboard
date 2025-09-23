<script setup>
const uploadedFiles = ref([])

function handleFiles(event) {
  const input = event.target 
  if (input.files) {
    uploadedFiles.value = [...uploadedFiles.value, ...Array.from(input.files)]
  }
}
</script>

<template>
  <VContainer>
    <VCard>
      <VCardItem>
        <VCardTitle>File Dropzone</VCardTitle>
        <VCardSubtitle>Upload files by dropping them below</VCardSubtitle>
      </VCardItem>

      <VCardText>
        <div
          class="border-dashed pt-2 border-2 border-primary rounded-lg p-8 text-center cursor-pointer"
          @drop.prevent="handleFiles"
          @dragover.prevent
        >
          <VIcon
            icon="tabler-upload"
            size="40"
            class="mb-2"
          />
          <p class="mb-2">
            Drag & Drop files here
          </p>
          <VBtn
            color="primary"
            @click="$refs.fileInput.click()"
          >
            Browse
          </VBtn>
          <p class="mb-2" />
          <input
            ref="fileInput"
            type="file"
            multiple
            class="hidden"
            @change="handleFiles"
          >
        </div>

        <!-- Show uploaded files -->
        <ul class="mt-4">
          <li
            v-for="(file, index) in uploadedFiles"
            :key="index"
          >
            {{ file.name }} ({{ (file.size / 1024).toFixed(1) }} KB)
          </li>
        </ul>
      </VCardText>
    </VCard>
  </VContainer>
</template>

<style scoped>
.hidden{
  display: none;

}
</style>
