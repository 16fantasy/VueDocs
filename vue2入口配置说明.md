# Vue2 工程核心入口配置总结

## 一、代码背景

以下代码通常位于 Vue2 项目的核心入口文件中，一般是：

```js
src/main.js
```

示例代码如下：

```js
import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import App from './App.vue';
import router from './router';
import './styles/global.css';

Vue.config.productionTip = false;
Vue.use(ElementUI);

new Vue({
  router,
  render: h => h(App)
}).$mount('#app');
```

这段代码是 Vue2 项目的启动入口，主要作用是：

> 初始化 Vue 应用，引入全局依赖，注册 Element UI，加载路由配置，渲染根组件 App.vue，并将整个应用挂载到页面中的 `#app` 节点上。

---

## 二、核心代码说明

### 1. 引入 Vue

```js
import Vue from 'vue';
```

这行代码用于引入 Vue 框架核心对象。

在 Vue2 中，需要通过 `new Vue()` 创建 Vue 实例，因此必须先引入 Vue。

---

### 2. 引入 Element UI 组件库

```js
import ElementUI from 'element-ui';
```

这行代码用于引入 Element UI 组件库。

Element UI 是 Vue2 中常用的 UI 框架，提供了按钮、表格、表单、弹窗、分页、日期选择器等常用组件。

常见组件示例：

```html
<el-button>按钮</el-button>
<el-table></el-table>
<el-dialog></el-dialog>
```

---

### 3. 引入 Element UI 样式

```js
import 'element-ui/lib/theme-chalk/index.css';
```

这行代码用于引入 Element UI 的默认主题样式。

如果只引入 Element UI 组件库，而没有引入对应 CSS 样式，页面上的按钮、表格、弹窗等组件可能无法正常显示样式。

---

### 4. 引入根组件 App.vue

```js
import App from './App.vue';
```

这行代码用于引入项目的根组件 `App.vue`。

`App.vue` 是整个 Vue 应用的最外层组件，可以理解为整个前端项目的页面容器。

通常在 `App.vue` 中会配置：

```html
<router-view />
```

用于显示不同路由对应的页面组件。

---

### 5. 引入路由配置

```js
import router from './router';
```

这行代码用于引入项目的路由配置。

路由配置通常位于：

```js
src/router/index.js
```

它负责管理不同访问路径与页面组件之间的映射关系，例如：

```text
/login
/home
/user/list
```

这些路径分别对应不同的 Vue 页面组件。

---

### 6. 引入全局样式

```js
import './styles/global.css';
```

这行代码用于引入项目的全局 CSS 样式。

全局样式一般用于定义整个系统通用的页面样式，例如：

```css
body {
  margin: 0;
}

.page-container {
  padding: 20px;
}
```

这些样式会作用于整个 Vue 项目。

---

## 三、Vue 全局配置说明

### 1. 关闭生产环境提示

```js
Vue.config.productionTip = false;
```

这行代码用于关闭 Vue 在浏览器控制台中的生产环境提示信息。

默认情况下，Vue 在开发环境中可能会在控制台输出类似提示：

```text
You are running Vue in development mode...
```

设置为 `false` 后，可以减少控制台中的无关提示信息。

---

## 四、注册 Element UI

```js
Vue.use(ElementUI);
```

这行代码用于将 Element UI 注册到 Vue 全局中。

注册完成后，项目中所有 Vue 组件都可以直接使用 Element UI 提供的组件，例如：

```html
<el-button type="primary">保存</el-button>
<el-input v-model="name"></el-input>
<el-table :data="tableData"></el-table>
```

这样就不需要在每个页面中单独引入 Element UI 组件。

---

## 五、创建 Vue 实例并挂载应用

### 1. 创建 Vue 根实例

```js
new Vue({
  router,
  render: h => h(App)
}).$mount('#app');
```

这部分是整个 Vue2 应用启动的核心代码。

---

### 2. 注入路由对象

```js
router,
```

这是一种 JavaScript 简写方式，等价于：

```js
router: router
```

它的作用是将路由对象挂载到 Vue 实例中。

挂载之后，项目中就可以使用 Vue Router 的相关能力，例如：

```js
this.$router.push('/home');
this.$route.params;
```

同时，也可以在组件模板中使用：

```html
<router-view />
<router-link />
```

---

### 3. 渲染根组件 App

```js
render: h => h(App)
```

这行代码的作用是将 `App.vue` 渲染成页面内容。

它等价于下面的完整写法：

```js
render: function (createElement) {
  return createElement(App);
}
```

其中 `h` 是 `createElement` 的简写，用于创建虚拟 DOM 节点。

简单理解就是：

> 把 `App.vue` 作为整个项目的根组件渲染出来。

---

### 4. 挂载到页面 DOM 节点

```js
.$mount('#app');
```

这行代码表示将 Vue 应用挂载到 HTML 页面中 `id="app"` 的 DOM 节点上。

通常在 `public/index.html` 中会有如下代码：

```html
<div id="app"></div>
```

Vue 应用启动后，会把 `App.vue` 渲染出来的内容挂载到这个 `div` 中。

---

## 六、项目启动流程

整个 Vue2 应用的启动流程可以概括为：

```text
浏览器加载 index.html
        ↓
找到 <div id="app"></div>
        ↓
执行 main.js
        ↓
引入 Vue、Element UI、App.vue、router、全局 CSS
        ↓
注册 Element UI
        ↓
创建 Vue 根实例
        ↓
渲染 App.vue
        ↓
挂载到 #app
        ↓
Vue2 应用启动完成
```

---

## 七、总结

这段 Vue2 入口代码的主要作用是：

1. 引入 Vue 框架核心对象。
2. 引入并注册 Element UI 组件库。
3. 引入 Element UI 默认样式。
4. 引入项目根组件 `App.vue`。
5. 引入项目路由配置 `router`。
6. 引入全局 CSS 样式。
7. 创建 Vue 根实例。
8. 将根组件渲染到页面中的 `#app` 节点。

通俗来说，这段代码就是 Vue2 项目的“启动器”或“总入口”，负责把整个前端应用运行起来。
