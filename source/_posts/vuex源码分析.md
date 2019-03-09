---
title: '''vuex源码分析'''
copyright: true
permalink: 1
top: 0
date: 2019-03-02 20:14:19
categories:
tags:
---

## 一.启动一个 vue 项目

### 1.安装 vue 脚手架

```
npm install -g @vue/cli
```

or

```
yarn global add @vue/cli
```

### 2.创建项目

```
vue create vuex-imp
```

### 3.下载依赖包

```
npm install
```

### 4.运行项目

```
npm run serve
```

or

```
yarn serve
```

## 二.一步一步实现 vuex

### 1.store 中 state 的 使用

#### 1.新建 store.js 文件

```
import Vue from "vue";
import Vuex from "vuex";
Vue.use(Vuex);
export default new Vuex.Store({
  state: {
    count: 100
  }
});
```

#### 2.在 main.js 中添加 store

```
import Vue from "vue";
import App from "./App.vue";
import store from "./store";

Vue.config.productionTip = false;

new Vue({
  store,
  render: h => h(App)
}).$mount("#app");
```

#### 3.在 App.vue 中使用 store 中定义的 count 变量

```
<template>
  <div id="app">
    {{this.$store.state.count}}
  </div>
</template>

<script>
export default {
  name: "app"
};
</script>

<style>

</style>
```

* 打开浏览器页面出现定义的变量：100

### 2.自己实现 store 中的 state

#### 1.在 store.js 中新建 vuex.js 文件`Vue.use(Vuex);`会调用 Vuex 中的`install`方法,声明一个变量`Vue`,保留用户的构造函数

```
let Vue;

class Store {}
let install = _Vue => {
  Vue = _Vue;
};
export default {
  Store,
  install
};
```

#### 2.混合`mixin`,可以给所有的实列混合些东西，`beforeCreate`所有的组件创建之前都可以调用

```
let Vue;

class Store {}
let install = _Vue => {
  Vue = _Vue;
  Vue.mixin({
    beforeCreate() {
      console.log("aaa");
    }
  });
};
export default {
  Store,
  install
};
```

* 调用两次，第一次是 main.js 调用，第二次是 App.vue 组件调用

#### 3.我们需要把根组件中 store 实列给每个组件都增加一个$store

在 vuex.js 中

```
beforeCreate() {
  if (this.$options && this.$options.store) {
    this.$store = this.$options.store;
  } else {
    this.$store = this.$parent && this.$parent.$store;
  }
}
```

* 先判断是否是根组件，然后子组件根据父组件的 store 拿到 store

#### 4.组件中调用自己写的 vuex 方法，看是否有 store

在 App.js 中

```
<template>
  <div id="app">
    <!-- {{this.$store.state.count}} -->
  </div>
</template>

<script>
export default {
  name: "app",
  mounted() {
    console.log(this.$store);
  }
};
</script>

<style>

</style>
```

* 打开浏览器控制台显示：Store {}

#### 5.获取 state 中的值

* 组件中的 Store 是从 vuex 中 new 出来的`new Vuex.Store`

- Store 类中有 `state` `getters` `mutations` `actions` 等属性，可以通过 `state` 属性拿到 `state` 中的值

```
class Store {
  constructor(options) {
    let state = options.state;
    this._vm = new Vue({
      data: {
        state
      }
    });
  }
  get state() {
    return this._vm.state;
  }
}
```

* 什么样的属性可以实现双向数据绑定 有`get`和`set`如，`new vue({data:{}})`

- 查看浏览器：100

### 3.store 中 getters 属性

#### 1.getter 的使用

sotre.js 中

```
import Vue from "vue";
import Vuex from "vuex";
Vue.use(Vuex);
export default new Vuex.Store({
  state: {
    count: 100
  },
  getters: {
    newCount(state) {
      return state.count + 100;
    }
  }
});
```

App.vue 中

```
<template>
  <div id="app">
    {{this.$store.state.count}}
    {{this.$store.getters.newCount}}
  </div>
</template>

<script>
export default {
  name: "app",
  mounted() {}
};
</script>

<style>

</style>
```

* 浏览器页面显示：100 200

### 4.store 中 getters 实现的原理

#### 1.将 options 中的 getters 拿出来遍历这个对象，将值返回

```
let Vue;

class Store {
  constructor(options) {
    let state = options.state;
    this.getters = {};
    this.mutations = {};
    this.actions = {};
    this._vm = new Vue({
      data: {
        state
      }
    });
    if (options.getters) {
      let getters = options.getters;
      forEach(getters, (getterName, getterFn) => {
        Object.defineProperty(this.getters, getterName, {
          get: () => {
            return getterFn(state);
          }
        });
      });
    }
  }
  get state() {
    return this._vm.state;
  }
}
function forEach(obj, callback) {
  Object.keys(obj).forEach(item => callback(item, obj[item]));
}
let install = _Vue => {
  Vue = _Vue;
  Vue.mixin({
    beforeCreate() {
      if (this.$options && this.$options.store) {
        this.$store = this.$options.store;
      } else {
        this.$store = this.$parent && this.$parent.$store;
      }
    }
  });
};
export default {
  Store,
  install
};
```

* 页面出现：100 200

### 5.store 中 mutations 属性

#### 1.mutations 的用法

store.js

```
import Vue from "vue";
import Vuex from "vuex";
Vue.use(Vuex);
export default new Vuex.Store({
  state: {
    count: 100
  },
  getters: {
    newCount(state) {
      return state.count + 100;
    }
  },
  mutations: {
    change(state) {
      state.count += 10;
    }
  }
});
```

App.js

```
<template>
  <div id="app">
    {{this.$store.state.count}}
    {{this.$store.getters.newCount}}
    <button @click="change">add</button>
  </div>
</template>

<script>
export default {
  name: "app",
  mounted() {},
  methods: {
    change() {
      this.$store.commit("change");
    }
  }
};
</script>

<style>

</style>
```

* 点击按钮页面数字加 10

### 6.store 中 mutations 实现的原理

Vuex.js

```
class Store {
  constructor(options) {
    let state = options.state;
    this.getters = {};
    this.mutations = {};
    this.actions = {};
    this._vm = new Vue({
      data: {
        state
      }
    });
    if (options.getters) {
      let getters = options.getters;
      forEach(getters, (getterName, getterFn) => {
        Object.defineProperty(this.getters, getterName, {
          get: () => {
            return getterFn(state);
          }
        });
      });
    }
    let mutations = options.mutations;
    forEach(mutations, (mutationName, mutationFn) => {
      this.mutations[mutationName] = () => {
        mutationFn.call(this, state);
      };
    });
  }
  get state() {
    return this._vm.state;
  }
  commit(type) {
    this.mutations[type]();
  }
}
```

* 刷新浏览器页面数字加 10

### 7.store 中 action 属性

#### 1.action 的用法（异步方法）

store.js

```
actions: {
  change({ commit }) {
    setTimeout(() => {
      commit("change");
    }, 1000);
  }
}
```

App.js

```
change() {
  this.$store.dispatch("change");
}
```

* 点击浏览器按钮 1 秒后加 10

### 8.store 中的 action 属性的实现

store.js

```
class Store {
  constructor(options) {
    let state = options.state;
    this.getters = {};
    this.mutations = {};
    this.actions = {};
    this._vm = new Vue({
      data: {
        state
      }
    });
    if (options.getters) {
      let getters = options.getters;
      forEach(getters, (getterName, getterFn) => {
        Object.defineProperty(this.getters, getterName, {
          get: () => {
            return getterFn(state);
          }
        });
      });
    }
    let mutations = options.mutations;
    forEach(mutations, (mutationName, mutationFn) => {
      this.mutations[mutationName] = () => {
        mutationFn.call(this, state);
      };
    });
    let actions = options.actions;
    forEach(actions, (actionName, actionFn) => {
      this.actions[actionName] = () => {
        actionFn.call(this, this);
      };
    });
    let { commit, dispatch } = this;
    this.commit = type => {
      commit.call(this, type);
    };
    this.dispatch = type => {
      dispatch.call(this, type);
    };
  }
  get state() {
    return this._vm.state;
  }
  commit(type) {
    this.mutations[type]();
  }
  dispatch(type) {
    this.actions[type]();
  }
}
```

* 打开浏览器点击按钮数据加 10

### 9.store 中的 modules 属性的使用

store.js

```
import Vue from "vue";
import Vuex from "vuex";
Vue.use(Vuex);
export default new Vuex.Store({
  modules: {
    a: {
      state: {
        count: 200
      },
      modules: {
        b: {
          state: {
            count: 300
          }
        }
      }
    }
  },
  state: {
    count: 100
  },
  getters: {
    newCount(state) {
      return state.count + 100;
    }
  },
  mutations: {
    change(state) {
      state.count += 10;
    }
  },
  actions: {
    change({ commit }) {
      setTimeout(() => {
        commit("change");
      }, 1000);
    }
  }
});
```

App.vue

```
<template>
  <div id="app">
    {{this.$store.state.count}}
    {{this.$store.getters.newCount}}
    {{this.$store.state.a.b.count}}
    <button @click="change">add</button>
  </div>
</template>

<script>
export default {
  name: "app",
  mounted() {},
  methods: {
    change() {
      this.$store.dispatch("change");
    }
  }
};
</script>

<style>

</style>
```

* 打开浏览器会有 300 显示出来

### 10.store 中的 modules 属性实现的原理
