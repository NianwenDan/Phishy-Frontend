<script>
export default {
  name: "About",
  data() {
    return {
      version: '8.10.68.14'
    }
  }
}
</script>

<template>
  <div class="setting-title">
    Phishy browser extension version {{ version }}
  </div>
</template>

<style scoped>
.setting-title {
  font-size: 15px;
  margin-bottom: 6px;
}

.setting-description {
  font-size: 13px;
  color: #666;
  max-width: 600px;
}
</style>