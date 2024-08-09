## 模版

```vue
.vue文件
<template>
  <div>{{ message }}</div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello, Vue!'
    };
  },
  beforeCreate() {
    console.log('beforeCreate: 实例初始化之前');
  },
  created() {
    console.log('created: 实例创建完成');
  },
  beforeMount() {
    console.log('beforeMount: 挂载之前');
  },
  mounted() {
    console.log('mounted: 挂载完成，可以操作 DOM');
  },
  beforeUpdate() {
    console.log('beforeUpdate: 数据更新前');
  },
  updated() {
    console.log('updated: 数据更新后');
  },
  beforeDestroy() {
    console.log('beforeDestroy: 组件销毁前');
  },
  destroyed() {
    console.log('destroyed: 组件销毁后');
  }
};
</script>
<style>
</style>
```



## 生命周期钩子函数

1. `beforeCreate`：在实例初始化之前被调用，此时数据观测和事件配置尚未完成，不能访问 `data` 、 `computed` 、 `methods` 等。
2. `created`：实例已经创建完成，此时可以访问 `data` 、 `computed` ，但尚未挂载到 DOM ，无法操作 DOM 。
3. `beforeMount`：在挂载之前被调用，相关的 `render` 函数首次被调用。
4. `mounted`：组件挂载到 DOM 后调用，此时可以操作 DOM 。
5. `beforeUpdate`：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。
6. `updated`：数据更新导致的虚拟 DOM 重新渲染和打补丁之后调用。
7. `beforeDestroy`：组件销毁前调用。
8. `destroyed`：组件销毁后调用。

## 全局变量

```js
// store.js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    count: 0
  },
  getters: {
    doubleCount: state => state.count * 2
  },
  mutations: {
    increment(state) {
      state.count++;
    }
  },
  actions: {
    incrementAsync({ commit }) {
      setTimeout(() => {
        commit('increment');
      }, 1000);
    }
  }
});
```

