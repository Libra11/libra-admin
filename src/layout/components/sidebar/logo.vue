<script setup lang="ts">
import { useNav } from "@/layout/hooks/useNav";

const props = defineProps({
  collapse: Boolean
});

const { title } = useNav();
</script>

<template>
  <div class="sidebar-logo-container" :class="{ collapses: props.collapse }">
    <transition name="sidebarLogoFade">
      <router-link
        v-if="props.collapse"
        key="props.collapse"
        :title="title"
        class="sidebar-logo-link"
        to="/"
      >
        <FontIcon icon="team-iconlogo" svg style="width: 35px; height: 35px" />
      </router-link>
      <router-link
        v-else
        key="expand"
        :title="title"
        class="sidebar-logo-link"
        to="/"
      >
        <FontIcon icon="team-iconlogo" svg style="width: 35px; height: 35px" />
      </router-link>
    </transition>
  </div>
</template>

<style lang="scss" scoped>
.sidebar-logo-container {
  padding: 12px 0;
  border-bottom: 1px solid #eee;

  .sidebar-logo-link {
    width: 48px;
    height: 48px;
    color: $subMenuActiveText;
    display: flex;
    justify-content: center;
    align-items: center;

    .sidebar-title {
      display: block;
      width: 160px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      text-align: left;
      color: #1890ff;
      font-weight: 600;
      font-size: 20px;
      margin-top: 10px;
      font-family: Avenir, Helvetica Neue, Arial, Helvetica, sans-serif;
    }
  }

  .collapses {
    .sidebar-logo {
      margin-right: 0;
    }
  }
}
</style>
