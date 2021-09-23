<script setup>
  import { ref } from 'vue'

	const hashHex = ref('')

  function handleUpload(ev) {
    const reader = new FileReader()

    reader.onload = async (ev) => {
      const buffer = ev.target.result
      const hashBuffer = await crypto.subtle.digest('SHA-256', buffer);
      const hashArray = Array.from(new Uint8Array(hashBuffer));                     // convert buffer to byte array
      hashHex.value = hashArray.map(b => b.toString(16).padStart(2, '0')).join('')
    }

    reader.readAsArrayBuffer(ev.target.files[0])
  }
</script>

<template>
  <input type="file" @change="handleUpload($event)">
  <p v-if="hashHex">
    sha-256 file hash: {{hashHex}}
  </p>
</template>