<template>
  <div
      v-for="item in menuItems"
      :key="item.name"
      :class="['menu-item', { active: activeMenuItem === item.name }]"
      @click="setActiveMenuItem(item.name)"
  >
    <div class="menu-icon" :style="{ backgroundColor: item.iconBg }">
      <n-icon size="18" color="white">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
          <path
              d="M368.5 240H272v-96.5c0-8.8-7.2-16-16-16s-16 7.2-16 16V240h-96.5c-8.8 0-16 7.2-16 16 0 4.4 1.8 8.4 4.7 11.3 2.9 2.9 6.9 4.7 11.3 4.7H240v96.5c0 4.4 1.8 8.4 4.7 11.3 2.9 2.9 6.9 4.7 11.3 4.7 8.8 0 16-7.2 16-16V272h96.5c8.8 0 16-7.2 16-16s-7.2-16-16-16z"
          />
        </svg>
      </n-icon>
    </div>
    <span>{{ item.label }}</span>
  </div>
</template>

<script>
import {NIcon} from 'naive-ui';

export default {
  name: 'SettingSidebar',
  props: ['activeMenuItem', 'menuItems', 'setActiveMenuItem'],
  components: {
    NIcon,
  }
}
</script>
<style scoped>
.menu-item {
  display: flex;
  align-items: center;
  padding: 12px 15px;
  border-radius: 7px;
  margin-top: 3px;
  cursor: pointer;
  gap: 12px;
  transition: background-color 0.2s;
}

.menu-item:hover {
  background-color: rgba(0, 0, 0, 0.15);
}

.menu-item.active {
  background-color: rgba(0, 0, 0, 0.15);
}

.menu-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

</style>