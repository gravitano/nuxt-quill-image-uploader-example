<script setup lang="ts">
import ImageUploader from 'quill-image-uploader';

const content = ref('Hello World');

const modules = ref({
  name: 'imageUploader',
  module: ImageUploader,
  options: {
    upload: (file) => {
      console.log(file);
      return new Promise((resolve, reject) => {
        const formData = new FormData();
        formData.append('image', file);

        $fetch('/upload-image', formData)
          .then((res) => {
            console.log(res);
            resolve(res.data.url);
          })
          .catch((err) => {
            reject('Upload failed');
            console.error('Error:', err);
          });
      });
    },
  },
});
</script>

<template>
  <div>
    <ClientOnly>
      <VQuillEditor
        name="content"
        v-model="content"
        :modules="modules"
        toolbar="full"
      />
    </ClientOnly>
  </div>
</template>
