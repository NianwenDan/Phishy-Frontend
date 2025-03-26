<script>
import {NButton, NSpace, NSwitch, NForm, NFormItem, NInput} from "naive-ui";

export default {
  name: "General",
  components: {NSpace, NSwitch, NButton, NForm, NFormItem, NInput },
  data() {
    return {
      generalSettings: {
        isActiveExternalApi: false,
        phishTankApiKey: '',
        openPhishApiKey: ''
      },
    }
  }
}
</script>

<template>
  <h3>External API Key Configuration</h3>
  <p class="setting-description">
    Enable this option to use a third-party API for phishing website detection. When activated, website URLs will be sent to the selected external service for real-time analysis. This may improve detection accuracy but could involve sharing browsing data with the API provider.
  </p>
  <n-form>
    <n-form-item label="Enable External API Usage: ">
      <n-switch v-model:value="generalSettings.isActiveExternalApi"/>
    </n-form-item>

    <n-form-item v-if="generalSettings.isActiveExternalApi" label="PhishTank API Key:">
      <n-input v-model:value="generalSettings.phishTankApiKey" type="text" />
    </n-form-item>

    <n-form-item v-if="generalSettings.isActiveExternalApi" label="OpenPhish API Key:">
      <n-input v-model:value="generalSettings.openPhishApiKey" type="text" />
    </n-form-item>
  </n-form>
  <h3>Export Configuration Settings</h3>
  <p class="setting-description">
    Use this option to download and save your current plugin settings. This allows you to back up your configuration or transfer it to another device easily. Settings will be exported as a file for future import.
  </p>
  <n-space>
    <n-button>Export</n-button>
    <n-button>Import</n-button>
  </n-space>
</template>