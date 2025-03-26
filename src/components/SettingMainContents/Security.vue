<script>
import { NDataTable, NButton, NSpace, NModal, NForm, NFormItem, NInput, NSelect, NTag } from "naive-ui";
import { h } from "vue";
export default {
  name: "Security",
  components: {
    NInput, NFormItem, NForm, NSelect,
    NDataTable,
    NButton,
    NSpace,
    NModal,
    NTag
  },
  data() {
    return {
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
      blackAndWhitelistDataTable: {
        columns: [
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
                        type: "info",
                        bordered: false
                      },
                      { default: () => tagKey }
                  )
              );
              return tags;
            }
          }
        ],
        data: [
          {
            ruleId: "6DGfqP",
            pattern: "^/api/v1/users/\\d+$",
            action: ["Allow"]
          },
          {
            ruleId: "UUTSrG",
            pattern: "^/admin/.*$",
            action: ["Deny"]
          },
          {
            ruleId: "o1hvSr",
            pattern: "^/products/(\\d+|new)$",
            action: ["Allow"]
          }
        ]
      }
    }
  },
  methods: {
    addNewPolicy() {
      let newPolicy = {
        ruleId: this.securitySettings.createPolicy.ruleId,
        pattern: this.securitySettings.createPolicy.pattern,
        action: [this.securitySettings.createPolicy.action]
      }
      // add the new data to the table
      this.blackAndWhitelistDataTable.data.push(newPolicy);
      // close the dialog
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
}
</script>

<template>
  <h3>Blacklist & Whitelist</h3>
  <n-space>
    <n-button @click="securitySettings.isShowCreatePolicy = true">Create Policy</n-button>
    <n-button>Import Policy</n-button>
  </n-space>

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
        <n-select v-model:value="securitySettings.createPolicy.action" :options="securitySettings.createPolicy.actionOptions" />
      </n-form-item>
      <n-form-item>
        <n-button @click.prevent="addNewPolicy">Save</n-button>
      </n-form-item>
    </n-form>
  </n-modal>

  <n-data-table
      :single-line="false"
      :columns="blackAndWhitelistDataTable.columns"
      :data="blackAndWhitelistDataTable.data"
  />
</template>

<style scoped>
.n-data-table {
  margin-top: 10px;
}
</style>