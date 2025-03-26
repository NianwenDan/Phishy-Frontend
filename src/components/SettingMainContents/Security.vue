<script>
import {
  NDataTable,
  NButton,
  NSpace,
  NModal,
  NForm,
  NFormItem,
  NInput,
  NSelect,
  NTag
} from "naive-ui";
import { h } from "vue";

export default {
  name: "Security",
  components: {
    NInput,
    NFormItem,
    NForm,
    NSelect,
    NDataTable,
    NButton,
    NSpace,
    NModal,
    NTag
  },
  data() {
    return {
      currentView: "blacklist", // "blacklist", "whitelist", or "all"
      securitySettings: {
        isShowCreatePolicy: false,
        createPolicy: {
          bodyStyle: {
            width: "600px"
          },
          ruleId: this.generateRandomString(),
          pattern: '',
          action: '',
          actionOptions: [
            { label: 'Allow', value: 'Allow' },
            { label: 'Deny', value: 'Deny' }
          ]
        }
      },
      tableColumns: [ // added
        { title: "Rule Id", key: "ruleId" },
        { title: "Pattern", key: "pattern" },
        {
          title: "Action",
          key: "action",
          render(row) {
            const tags = row.action.map((tagKey) =>
              h(
                NTag,
                {
                  style: { marginRight: "6px" },
                  // added: success applies green and error applies red color to the button (auto)
                  type: tagKey === "Allow" ? "success" : "error", 
                  bordered: false
                },
                { default: () => tagKey }
              )
            );
            return tags;
          }
        }
      ],
      blacklistData: [ // added
        {
          ruleId: "UUTSrG",
          pattern: "^/admin/.*$",
          action: ["Deny"]
        },
        {
          ruleId: "CCtvsQ",
          pattern: "^/legituser/.*$",
          action: ["Deny"]
        }
      ],
      whitelistData: [ // added
        {
          ruleId: "6DGfqP",
          pattern: "^/api/v1/users/\\d+$",
          action: ["Allow"]
        },
        {
          ruleId: "o1hvSr",
          pattern: "^/products/(\\d+|new)$",
          action: ["Allow"]
        }
      ]
    };
  },
  methods: {
    addNewPolicy() {
      const newPolicy = {
        ruleId: this.securitySettings.createPolicy.ruleId,
        pattern: this.securitySettings.createPolicy.pattern,
        action: [this.securitySettings.createPolicy.action]
      };

      // modified logic to push into the correct list
      if (newPolicy.action.includes("Deny")) {
        this.blacklistData.push(newPolicy);
      } else {
        this.whitelistData.push(newPolicy);
      }

      this.securitySettings.isShowCreatePolicy = false;
      this.securitySettings.createPolicy.ruleId = this.generateRandomString();
      this.securitySettings.createPolicy.pattern = '';
      this.securitySettings.createPolicy.action = '';
    },
    generateRandomString(length = 6) {
      const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
      let result = '';
      for (let i = 0; i < length; i++) {
        result += chars.charAt(Math.floor(Math.random() * chars.length));
      }
      return result;
    }
  }
};
</script>

<template>
  <h3></h3>

  <!-- added buttons to switch views -->
  <!-- Top-left: blacklist & whitelist, Top-right: view all -->
  <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
  <!-- Left buttons -->
  <n-space>
    <n-button
      :type="currentView === 'blacklist' ? 'primary' : 'default'"
      @click="currentView = 'blacklist'"
    >
      Blacklist
    </n-button>
    <n-button
      :type="currentView === 'whitelist' ? 'primary' : 'default'"
      @click="currentView = 'whitelist'"
    >
      Whitelist
    </n-button>
  </n-space>

  <!-- Right button -->
  <n-button
    :type="currentView === 'all' ? 'primary' : 'default'"
    @click="currentView = 'all'"
  >
    View All
  </n-button>
  </div>

  <!-- table switching logic -->
  <n-data-table
    :single-line="false"
    :columns="tableColumns"
    :data="currentView === 'blacklist'
          ? blacklistData
          : currentView === 'whitelist'
          ? whitelistData
          : [...blacklistData, ...whitelistData]"
  />

  <!-- Bottom-right: Create & Import policy buttons -->
  <div style="display: flex; justify-content: flex-end; margin-top: 12px">
    <n-space>
      <n-button @click="securitySettings.isShowCreatePolicy = true">Create Policy</n-button>
      <n-button>Import Policy</n-button>
    </n-space>
  </div>

  <n-modal
    v-model:show="securitySettings.isShowCreatePolicy"
    :mask-closable="true"
    :bordered="true"
    preset="card"
    title="Create Policy"
    :style="securitySettings.createPolicy.bodyStyle"
  >
    <n-form ref="formRef">
      <n-form-item label="Rule Id: ">
        <n-input v-model:value="securitySettings.createPolicy.ruleId" type="text" disabled />
      </n-form-item>

      <n-form-item label="Pattern: ">
        <n-input v-model:value="securitySettings.createPolicy.pattern" type="text" clearable />
      </n-form-item>

      <n-form-item label="Action: ">
        <n-select
          v-model:value="securitySettings.createPolicy.action"
          :options="securitySettings.createPolicy.actionOptions"
        />
      </n-form-item>

      <n-form-item>
        <n-button @click.prevent="addNewPolicy">Save</n-button>
      </n-form-item>
    </n-form>
  </n-modal>


</template>

<style scoped>
.n-data-table {
  margin-top: 10px;
}
</style>
