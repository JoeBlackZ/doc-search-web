<template>
  <a-layout>
    <a-layout-sider v-model:collapsed="collapsed" :trigger="null" collapsible>
      <div class="logo"/>
      <a-menu v-model:selectedKeys="selectedKeys" theme="dark" mode="inline">
        <a-menu-item key="1" @click="addTab()">
          <user-outlined/>
          <span>首页</span>
        </a-menu-item>
        <a-menu-item key="2">
          <video-camera-outlined/>
          <span>文档管理</span>
        </a-menu-item>
        <a-menu-item key="3">
          <upload-outlined/>
          <span>系统设置</span>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>
    <a-layout>
      <a-layout-header class="layout-header">
        <menu-unfold-outlined v-if="collapsed" class="trigger" @click="() => (collapsed = !collapsed)"/>
        <menu-fold-outlined v-else class="trigger" @click="() => (collapsed = !collapsed)"/>
      </a-layout-header>
      <a-layout-content class="layout-content">
        <a-tabs v-model:activeKey="activeKey" type="editable-card" @edit="onEdit">
          <a-tab-pane v-for="pane in panes" :key="pane.key" :tab="pane.title" :closable="pane.closable">
            {{ pane.content }}
          </a-tab-pane>
        </a-tabs>
      </a-layout-content>
    </a-layout>
  </a-layout>
</template>

<script>
import {
  UserOutlined,
  VideoCameraOutlined,
  UploadOutlined,
  MenuUnfoldOutlined,
  MenuFoldOutlined
} from '@ant-design/icons-vue';
import {defineComponent, ref} from 'vue';

export default defineComponent({
  name: "MainLayout",
  components: {
    UserOutlined,
    VideoCameraOutlined,
    UploadOutlined,
    MenuUnfoldOutlined,
    MenuFoldOutlined,
  },
  setup() {
    const panes = ref([
      {title: 'Tab 1', content: 'Content of Tab 1', key: '1'},
      {title: 'Tab 2', content: 'Content of Tab 2', key: '2',},
      {title: 'Tab 3', content: 'Content of Tab 3', key: '3', closable: false,}
    ]);
    const activeKey = ref(panes.value[0].key);
    const newTabIndex = ref(0);
    const add = () => {
      activeKey.value = `newTab${++newTabIndex.value}`;
      panes.value.push({title: 'New Tab', content: 'Content of new Tab', key: activeKey.value,});
    };
    const remove = targetKey => {
      let lastIndex = 0;
      panes.value.forEach((pane, i) => {
        if (pane.key === targetKey) {
          lastIndex = i - 1;
        }
      });
      panes.value = panes.value.filter(pane => pane.key !== targetKey);
      if (panes.value.length && activeKey.value === targetKey) {
        if (lastIndex >= 0) {
          activeKey.value = panes.value[lastIndex].key;
        } else {
          activeKey.value = panes.value[0].key;
        }
      }
    };
    const onEdit = (targetKey, action) => {
      if (action === 'add') {
        add();
      } else {
        remove(targetKey);
      }
    };
    return {
      panes,
      activeKey,
      onEdit,
      selectedKeys: ref(['1']),
      collapsed: ref(false),
    };
  },
  methods:{
    addTab: function () {
      console.info(1)
    }
  }
});
</script>
<style>

.ant-layout {
  height: 100%;
}

.trigger {
  font-size: 18px;
  line-height: 64px;
  padding: 0 24px;
  cursor: pointer;
  transition: color 0.3s;
}

.trigger:hover {
  color: #1890ff;
}

.layout-content {
  margin: 10px 5px;
  padding: 6px;
  border: 1px solid #b2afaf;
  background: #fff;
}

.layout-header {
  background: #fff !important;
  padding: 0 !important;
}

.logo {
  height: 32px;
  background: rgba(255, 255, 255, 0.3);
  margin: 16px;
}

</style>