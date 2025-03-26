<template>
  <ContentLayout>
    <template v-slot:sidebar>
      <setting-sidebar
          :active-menu-item="activeMenuItem"
          :menu-items="menuItems"
          :set-active-menu-item="setActiveMenuItem">
      </setting-sidebar>
    </template>
    <template v-slot:main-content-label>{{ getActiveMenuLabel }}</template>
    <template v-slot:main-content>
      <component :is="activeMenuItem"></component>
    </template>
  </ContentLayout>
</template>

<script>
import SettingSidebar from "@/components/SettingSidebar.vue";
import ContentLayout from "@/layout/ContentLayout.vue";
import General from "@/components/SettingMainContents/General.vue"
import Security from "@/components/SettingMainContents/Security.vue";
import Audit from "@/components/SettingMainContents/Audit.vue";
import About from "@/components/SettingMainContents/About.vue";

export default {
  name: 'setting-content',
  components: {
    General,
    Security,
    Audit,
    About,
    ContentLayout,
    SettingSidebar
  },
  data() {
    return {
      activeMenuItem: 'general',
      menuItems: [
        {name: "general", label: "General", iconBg: "#54B4FF", iconName: 'SettingsOutline'},
        {name: "security", label: "Security", iconBg: "#F5A623", iconName: 'ShieldCheckmarkOutline'},
        {name: "audit", label: "Audit", iconBg: "#4CD964", iconName: 'DocumentTextOutline'},
        {name: "about", label: "About", iconBg: "#9E9E9E", iconName: 'InformationCircleOutline'},
      ]
    }
  },
  methods: {
    setActiveMenuItem(menuName) {
      this.activeMenuItem = menuName;
    }
  },
  computed: {
    getActiveMenuLabel() {
      for (const menuItem of this.menuItems) {
        if (this.activeMenuItem === menuItem.name) {
          return menuItem.label;
        }
      }
      return 'Failed to Find Current Active Label'
    }
  }
}
</script>