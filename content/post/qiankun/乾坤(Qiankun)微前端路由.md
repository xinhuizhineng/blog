---
title: "乾坤(Qiankun)微前端路由"
author: "常香玉"              # 文章作者
description : "乾坤(Qiankun)微前端实战：解决 Angular 子应用三级路由回退后导航失效问题"    # 文章描述信息
date: 2023-04-27T13:05:18+08:00
lastmod: 2023-04-27         # 文章修改日期
tags : [                    # 文章所属标签
    "Web前端",
    "Qiankun",
    "路由"
]
categories : [              # 文章所属标签
    "Web前端",
]

---

在微前端架构中，主应用与子应用的技术栈差异往往会带来一些意想不到的路由同步问题。本文将分享一个在 Vue3 主应用 + Angular 子应用 场景下，遇到的“三级路由回退导致导航失效”的各种坑及其解决方案。  

## 1. 问题描述
场景
  - 主应用：Vue3 + Vue Router (Hash 模式)
  - 子应用：Angular (使用 Qiankun 接入)
  - 操作路径：进入 Angular 子应用 -> 进入二级页面 -> 进入三级页面 -> 点击“返回”按钮回退到上一级。
症状
当从三级路由回退后，再次点击主应用侧边栏或其他菜单时，页面无反应。查看浏览器控制台，出现类似 `route undefined` 的报错。

## 2. 原因深度分析
经过源码排查，发现问题的根源在于 **Vue Router** 与 **Angular Router** 对底层 **History API** 的使用差异。

  1. **Vue Router 的机制**： Vue3 的路由跳转底层调用了 `changeLocation` 函数。该函数在执行时，高度依赖 `history.state` 来获取路由信息。如果 `history.state` 为空或缺少关键字段（如 `current`），Vue Router 可能无法正确识别当前位置，从而导致跳转逻辑中断。
  
  2. **Angular Router 的差异**： Angular 的默认路由跳转（特别是使用 HTML 中的 `[routerLink]="['../']"` 指令时），并不会默认向 `history.state` 中写入 Vue Router 所需的元数据。

结论：当在 Angular 子应用中进行路由回退时，由于没有正确更新 `history.state`，导致主应用（Vue3）接管路由时读取到了空状态或错误状态，最终引发路由报错。

## 3. 解决方案
为了解决这个问题，我们需要在“子应用跳转”和“主应用监听”两个层面进行改造。

### 步骤一：改造 Angular 子应用的跳转方式
**核心策略**：放弃 HTML 模板中的 `[routerLink]` 跳转，改用 API 编程式导航，并手动注入 `state`。

Angular 的 `Router.navigate` 方法允许我们传递 `NavigationExtras`，其中的 `state` 属性可以让我们自定义路由状态。这正好可以用来模拟 Vue Router 所需的数据结构。  
代码实现 (Angular 子应用)：
```
// 在组件中注入 Router
import { Router } from '@angular/router';

constructor(private _router: Router) {}

// 替代原本的 [routerLink]="['../']"
back() {
  // 手动构建 state 对象，模拟 Vue Router 的 current 字段
  // 注意：这里的路径需要根据实际业务调整
  const state = { current: '/kq/kqGroup' };
  
  // 使用 navigate 跳转，并带上 state
  this._router.navigate(['/kqGroup'], { state: state });
}
```
注意：建议三级路由跳转时，URL 中尽量不要直接携带参数，可以考虑通过 Service 或 State 传递数据，以减少路由匹配的复杂度。  

### 步骤二：改造 Vue3 主应用的路由守卫
**核心策略**：在主应用的全局路由守卫中，主动检测并修复 `history.state`。

我们需要在 `router.beforeEach` 中监听路由变化。如果发现 `history.state` 丢失或不完整（缺少 current 字段），则使用 `history.replaceState` 强制补全，确保 Vue Router 能正常工作。

代码实现 (Vue3 主应用)：
```
import { createRouter, createWebHashHistory } from 'vue-router';

const router = createRouter({
  history: createWebHashHistory(),
  routes,
});

router.beforeEach(async (to, from, next) => {
  // 1. 核心修复逻辑：补全 history.state
  // 如果 state 存在但没有 current 字段，说明可能是子应用过来的跳转，手动补全
  if (history.state && !history.state.current) {
    console.log('检测到 state 缺失，执行 replaceState 修复');
    window.history.replaceState({ current: to.path }, '', to.path);
  }

  // 调试日志
  console.log('微前端 history.state ===', history.state);
  console.log('微前端 window.location ===', window.location);

  // 2. 特殊业务场景修复（如巡更业务）
  // 修复通过菜单切换后，history.state 丢失应用前缀信息的问题
  const current = history.state?.current;
  const pathname = window.location.pathname;
  
  if (pathname && pathname.startsWith('/xg')) {
    if (current !== pathname) {
      history.replaceState(
        { back: from.path, current: window.location.pathname }, 
        to.path, 
        ''
      );
    }
  }

  // 3. 常规鉴权与路由分发逻辑
  if (!verifyToken() && to.path !== '/login') {
    // ... 处理登录逻辑
    if (noLoginPage.includes(to.path)) {
      next();
    } else {
      next({ path: '/login', query: { fromPath: to.fullPath } });
    }
  } else {
    // 清理缓存逻辑
    if (to.path === '/home') {
      sessionStorage.removeItem('currentMenu');
      sessionStorage.removeItem('currentPage');
    }

    // 动态路由加载逻辑
    if (store.state.menus.routers.length === 0 && to.path !== '/login') {
       // 处理菜单基合菜单列表
      const res = await store.dispatch('menus/getMenus');
      res.forEach((route) => {
        if (router.hasRoute('Main')) {
          router.addRoute('Main', route);
        }
      });
      const routers = router.getRoutes();
      // Tips不检查门禁监控菜单
      if (routers.find((item) => item.path === to.path || /\/monitor\/mj\//.test(to.path))) {
        next({ ...to, replace: true });
      } else {
        next({ path: '/not-find' });
      }
    } else {
       // 404 检测
       const routers = router.getRoutes();
       // 简单的白名单检查
       if (routers.find(item => item.path === to.path || /\/monitor\/mj\//.test(to.path))) {
         next();
       } else {
         next({ path: '/not-find' });
       }
    }
  }
});
```
## 4. 总结
在乾坤微前端架构中，不同框架对 `History API` 的操作习惯不同是导致路由不同步的常见原因。

- **Angular** 侧：利用 `navigate` 的 `state` 参数主动配合主应用的路由机制。
- **Vue** 侧：利用 `beforeEach` 守卫进行兜底，确保 `history.state` 始终包含 `current` 字段。
通过双向奔赴的改造，我们成功解决了三级路由回退后的“死锁”问题，保证了微前端应用的流畅体验。
