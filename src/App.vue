<template>
  <BaseLayout>
    <template v-slot:header>
      <setting-header></setting-header>
    </template>
    <template v-slot:content>
      <setting-content></setting-content>
    </template>
  </BaseLayout>
</template>

<script>
import BaseLayout from "@/layout/BaseLayout.vue";
import SettingHeader from "@/components/SettingHeader.vue";
import SettingContent from "@/components/SettingContent.vue";

export default {
  name: 'App',
  components: {
    BaseLayout,
    SettingHeader,
    SettingContent,
  },
}
</script>

<style scoped>
.app-container {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  max-width: 1100px;
  margin: 0 auto;
  padding-top: 35px;
}

.settings-container {
  overflow: hidden;
}
</style>