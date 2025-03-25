<script>
import { NButton, NSwitch } from "naive-ui";

export default {
  name: "General",
  components: { NSwitch, NButton },
  data() {
    return {
      settings: {
        integrateDesktop: true
      },
    }
  }
}
</script>

<template>
<!--  <div class="setting-item">-->
<!--    <div>-->
<!--      <div class="setting-title">This is a title text</div>-->
<!--      <div class="setting-description">-->
<!--        There are some descriptions.-->
<!--      </div>-->
<!--    </div>-->
<!--    <n-switch v-model:value="settings.integrateDesktop"/>-->
<!--  </div>-->
  <h3>Export Configuration Settings</h3>
  <n-button>Export</n-button>
</template>