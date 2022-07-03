<template>
  <a-menu
    v-model:openKeys="openKeys"
    mode="inline"
    :selected-keys="[selectedKey]"
    :style="{ height: '100%', borderRight: 0 }"
    class="nav"
  >
    <a-sub-menu key="/doc/guide">
      <template #title>{{ isZhCN ? '必读' : 'Guide' }}</template>
      <a-menu-item :key="`/doc/guide#${isZhCN ? '简介' : 'introduction'}`">
        <router-link :to="`/doc/guide#${isZhCN ? '简介' : 'introduction'}`">
          {{ isZhCN ? '简介' : 'Introduction' }}
        </router-link>
      </a-menu-item>
      <a-menu-item :key="`/doc/guide#${isZhCN ? '快速开始' : 'Start'}`">
        <router-link :to="`/doc/guide#${isZhCN ? '快速开始' : 'Start'}`">
          {{ isZhCN ? '快速开始' : 'Getting Started' }}
        </router-link>
      </a-menu-item>
      <a-menu-item :key="`/doc/guide#${isZhCN ? '常见问题' : 'FAQ'}`">
        <router-link :to="`/doc/guide#${isZhCN ? '常见问题' : 'FAQ'}`">
          {{ isZhCN ? '常见问题' : 'FAQ' }}
        </router-link>
      </a-menu-item>
    </a-sub-menu>
    <a-menu-item :key="`/doc/api`">
      <router-link :to="`/doc/api`">API</router-link>
    </a-menu-item>
    <a-menu-item :key="`/doc/changelog`">
      <router-link :to="`/doc/changelog`">{{ isZhCN ? '更新日志' : 'Change Log' }}</router-link>
    </a-menu-item>
    <template v-for="demo in demos" :key="'/doc/' + demo.type">
      <a-sub-menu
        v-if="true"
        :key="'/doc/' + demo.type"
        :title="demo[isZhCN ? 'title' : 'enTitle']"
      >
        <a-menu-item v-if="true" :key="'/doc/' + demo.type">
          <router-link :to="`/doc/${demo.type}`">
            {{ isZhCN ? '概述' : 'Overview' }}
          </router-link>
        </a-menu-item>
        <template v-for="item in demo.children || []" :key="item.id">
          <a-menu-item
            v-if="true"
            :key="'/doc/' + demo.type + '#' + item.id"
            :disabled="item.disabled"
          >
            <router-link :to="`/doc/${demo.type}#${item.id}`" :disabled="item.disabled">
              {{ item[isZhCN ? 'title' : 'enTitle'] }}{{ item.disabled ? '（ing）' : '' }}
            </router-link>
          </a-menu-item>
        </template>
      </a-sub-menu>
    </template>
  </a-menu>
</template>
<script lang="ts">
import { defineComponent, toRef, ref } from 'vue';
import demos from '../demo/demos';
import { useInjectGlobalConfig } from '../context';
import { useRoute } from 'vue-router';

export default defineComponent({
  setup() {
    const globalConfig = useInjectGlobalConfig();
    const route = useRoute();
    return {
      demos,
      isZhCN: globalConfig.isZhCN,
      selectedKey: toRef(route, 'fullPath'),
      openKeys: ref<string[]>([...new Set(['/doc/guide', route.path])]),
    };
  },
});
</script>
<style lang="less" scoped>
.nav :deep(.ant-menu-item) {
  margin: 0;
}
</style>
