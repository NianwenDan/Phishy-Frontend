<script>
import { NDataTable, NButton, NSpace, NModal, NForm, NFormItem, NInput, NSelect, NTag, NButtonGroup } from "naive-ui";
import { h } from "vue";

export default {
  name: "Security",
  components: {
    NInput, NFormItem, NForm, NSelect, NDataTable,
    NButton, NSpace, NModal, NTag, NButtonGroup
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
      },
        {title: "Manage", key: "actions",
            render: (row, index) =>
                h( NButton,
                  { class: "hover-button", size: "small", type: "error",
                    onClick: () => this.deletePolicy(row, index)
                  },
                  { default: () => "Delete" }
                )
        }
      ],
      blacklistData: [ // temp data for bl
        { ruleId: "UUTSrG", pattern: "^/admin/.*$", action: ["Deny"] },
        { ruleId: "CCtvsQ", pattern: "^/legituser/.*$", action: ["Deny"] },
        { ruleId: "zzttvQ", pattern: "^/remoteuser/.*$", action: ["Deny"] }
      ],
      whitelistData: [ // temp data for wl
        { ruleId: "6DGfqP", pattern: "^/api/v1/users/\\d+$", action: ["Allow"]},
        { ruleId: "o1hvSr", pattern: "^/products/(\\d+|new)$", action: ["Allow"]}
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
      if (newPolicy.action.includes("Deny")) { this.blacklistData.push(newPolicy); } else {this.whitelistData.push(newPolicy);}

      this.securitySettings.isShowCreatePolicy = false;
      this.securitySettings.createPolicy.ruleId = this.generateRandomString();
      this.securitySettings.createPolicy.pattern = '';
      this.securitySettings.createPolicy.action = '';
    },
    // Policy(row) deletion logic
    deletePolicy(row) {
      const targetList = row.action.includes("Deny") ? this.blacklistData : this.whitelistData;
      const index = targetList.findIndex(item => item.ruleId === row.ruleId);
      if (index !== -1) targetList.splice(index, 1);
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
  <!-- Table with control group on the right side -->
  <div style="display: flex; gap: 20px; align-items: flex-start;">
     <!-- table switching logic: If in BL, view BL; If in WL, view WL; If view all, display all data -->
    <n-data-table
      style="flex: 1"
      :single-line="false"
      :columns="tableColumns"
      :data="currentView === 'blacklist'
            ? blacklistData
            : currentView === 'whitelist'
            ? whitelistData
            : [...blacklistData, ...whitelistData]"
    />
    
    <!-- Button group: Right side of the table -->
    <n-button-group vertical style="border-radius: 14px; overflow: hidden; box-shadow: 0 1px 3px rgba(0,0,0,0.1);">
      <n-button class="hover-button" :type="currentView === 'blacklist' ? 'primary' : 'default'" @click="currentView = 'blacklist'">Blacklist</n-button>
      <n-button class="hover-button" :type="currentView === 'whitelist' ? 'primary' : 'default'" @click="currentView = 'whitelist'">Whitelist</n-button>
      <n-button class="hover-button":type="currentView === 'all' ? 'primary' : 'default'" @click="currentView = 'all'">View All</n-button>
    </n-button-group>
  </div>

 <!-- Bottom-left: Create & Import policy buttons -->
 <div style="display: flex; justify-content: flex-start; margin-top: 12px">
  <n-space>
    <n-button class="hover-button-with-black" @click="securitySettings.isShowCreatePolicy = true">Create Policy</n-button>
    <n-button class="hover-button-with-black">Import Policy</n-button>
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



<!-- Bottom-left: Create & Import policy buttons -->
<style scoped>
.hover-button-with-black { 
  background-color: black;
  color: white;
  transition: all 0.3s ease;
  border-radius: 6px;
}
.hover-button-with-black:hover {
  transform: scale(1.05);
  border-radius: 12px;
}
:deep(.hover-button) {
  transition: all 0.3s ease;
  border-radius: 6px;
}
:deep(.hover-button:hover) {
  transform: scale(1.05);
  border-radius: 12px;
}

</style>