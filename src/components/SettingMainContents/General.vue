<script>
import {NBadge, NButton, NSwitch} from "naive-ui";

export default {
  name: "General",
  components: {NBadge, NSwitch, NButton},
  data() {
    return {
      settings: {
        defaultManager: true,
        integrateDesktop: true
      },
    }
  }
}
</script>

<template>
  <div class="setting-item">
    <div>
      <div class="setting-title">Make 1Password the default password manager in this browser</div>
    </div>
    <n-switch v-model:value="settings.defaultManager"/>
  </div>

  <div class="setting-item">
    <div>
      <div class="setting-title">Integrate this extension with the 1Password desktop app</div>
      <div class="setting-description">
        When the extension and the desktop app are connected, they lock and unlock together. Biometrics will be
        used if available.
      </div>
    </div>
    <n-switch v-model:value="settings.integrateDesktop"/>
  </div>

  <div class="integration-status">
    <span>Integration status: Connected</span>
    <n-badge dot type="success"/>
  </div>

  <div class="diagnostic-section">
    <div>
      <div class="setting-title">Generate diagnostics report for 1Password Support</div>
      <div class="setting-description text-small">
        The report contains detailed debugging and log information. No passwords or secure information are
        included.
      </div>
    </div>
    <n-button>Generate report</n-button>
  </div>
</template>

<style scoped>
.setting-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 0;
  border-bottom: 1px solid #EEEEEE;
}

.setting-title {
  font-size: 15px;
  margin-bottom: 6px;
}

.setting-description {
  font-size: 13px;
  color: #666;
  max-width: 600px;
}

.integration-status {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 16px 0;
  font-size: 14px;
  color: #666;
  border-bottom: 1px solid #EEEEEE;
}

.diagnostic-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 0;
}

.text-small {
  font-size: 12px;
}
</style>