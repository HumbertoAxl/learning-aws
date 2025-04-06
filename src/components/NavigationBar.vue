<template>
  <v-toolbar color="#0f0f33" title="AWS Learning Experience" class="toolbar">
    <v-toolbar-items class="ml-auto">
      <v-btn variant="text" @click.stop="showBuyMeACoffeeMessage()"> Click here to offer me a job </v-btn>
    </v-toolbar-items>
  </v-toolbar>
</template>

<script setup>
import Swal from 'sweetalert2';

function showBuyMeACoffeeMessage() {
  Swal.fire({
    title: 'Thanks!',
    text: 'When do I start?',
    icon: 'success',
    confirmButtonText: 'Next week!',
    customClass: {
      confirmButton: 'swal-button-white-text',
    },
  });
}
</script>

<style scoped>
.v-toolbar {
  color: white;
}

.swal-button-white-text {
  color: white !important;
}

.swal-toast-white-text {
  color: white !important;
}

.toolbar {
  padding-left: 15%;
  padding-right: 15%;
}
</style>
