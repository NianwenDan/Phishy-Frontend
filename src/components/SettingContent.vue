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
      <div v-if="activeMenuItem === 'general'">
        <general></general>
      </div>
      <div v-else-if="activeMenuItem === 'about'">
        <about></about>
      </div>
    </template>
  </ContentLayout>
</template>

<script>
import SettingSidebar from "@/components/SettingSidebar.vue";
import ContentLayout from "@/layout/ContentLayout.vue";
import General from "@/components/SettingMainContents/General.vue"
import About from "@/components/SettingMainContents/About.vue"

export default {
  name: 'setting-content',
  components: {
    About,
    ContentLayout,
    SettingSidebar,
    General
  },
  data() {
    return {
      activeMenuItem: 'general',
      menuItems: [
        { name: 'general', label: 'General', iconBg: '#54B4FF' },
        { name: 'security', label: 'Security', iconBg: '#F5A623' },
        { name: 'autofill', label: 'Autofill & save', iconBg: '#F5A623' },
        { name: 'accounts', label: 'Accounts & vaults', iconBg: '#4CD964' },
        { name: 'notifications', label: 'Notifications', iconBg: '#FF7EAE' },
        { name: 'watchtower', label: 'Watchtower', iconBg: '#9E9E9E' },
        { name: 'appearance', label: 'Appearance & shortcuts', iconBg: '#9E9E9E' },
        { name: 'integrations', label: 'Integrations', iconBg: '#4CD964' },
        { name: 'about', label: 'About', iconBg: '#54B4FF' }
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