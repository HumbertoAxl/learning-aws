<template>
  <v-toolbar color="#0f0f33" title="AWS Learning Experience" class="toolbar">
    <v-toolbar-items class="ml-auto">
      <v-btn variant="text" @click.stop="showBuyMeACoffeeMessage()"> Click here to offer me a job </v-btn>
    </v-toolbar-items>
  </v-toolbar>
</template>

<script setup>
function showBuyMeACoffeeMessage() {
  alert('Thanks, when do I start?');
}
</script>

<style scoped>
.v-toolbar {
  color: white;
}

.toolbar {
  padding-left: 15%;
  padding-right: 15%;
}
</style>
