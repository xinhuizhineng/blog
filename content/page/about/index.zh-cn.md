---
title: 关于
menu:
    main: 
        weight: -90
        url: "/about"
        params:
            icon: user
---

<div class="resume-wrapper">
    
# 常香玉 | 前端开发工程师 | 6 年+

## 基本信息

- 手机：17111124608
- 邮箱：changxiangyu@changxiangyu.cn
- GitHub 主页：https://github.com/changxiangyu666
- 作品组织：https://github.com/jinyumantangcjx，https://github.com/xinhuizhineng
- 毕业院校及专业：黑河学院 | 20届 | 计算机科学与技术专业（本科）
- 我的博客: https://changxiangyu.cn

## 核心技能
- 编程语言： JavaScript (ES6+), TypeScript, Dart, Python, Java, HTML5, CSS3
- 前端框架： Vue.js (Avue), Angular, qiankun (微前端), Ionic, amis
- 移动/小程序： uni-app, Flutter, 微信小程序/公众号, 企业微信/支付宝对接
- 后端技术： Node.js, Flask, MVC 架构
- 数据库： MySQL, Redis, MongoDB, SQL
- 可视化/GIS： ECharts, OpenLayers, 高德地图 API, Photoshop (切图/处理)
- 工具/运维： Git/GitHub/GitLab, Nginx, IIS, npm/yarn
- 相关知识: HTTP协议、浏览器工作原理。
- 能够独立从0到1搭建前端项目。



## 工作经历

### 达实集团 / 达实物联网 | 研发部-前端工程师

_2021.03 - 至今_

#### **改单类项目**

- **技术栈**：HTML5、CSS3、JavaScript、gulp 构建工具、uniapp（跨平台框架）、Vue、Angular、Axios（接口请求）、ES6、Promise 异步处理、WebSocket
- **功能模块**：
  - 支付系统：微信 H5、小程序、支付宝、银行卡支付
  - 考勤系统：基于高德地图定位签到
  - 访客与梯控：访客预约、梯控权限管理
  - **人脸识别门禁**：支持用户通过移动端上传/拍摄人脸照片 → 后端提取人脸特征并存储 → 门禁设备调用人脸识别算法实时比对 → 验证通过后自动开门
  - 数据大屏：自适应布局，实时展示数据
  - C3v8 访客机：硬件设备数据交互与界面展示
- **性能优化**：
  - 接口统一封装，支持异常处理与重试
  - 上传人脸照片时进行图片压缩与格式校验，控制文件大小 ≤ 500KB，减少网络延迟
  - 识别算法调用优化，准确率提升至 98%+，平均响应时间 < 1.5 秒
  - 资源文件按需加载，减少首屏时间
  - 前端缓存策略减少重复请求
- **部署与适配**：
  - 适配 iOS / Android ，微信小程序，微信公众号，企业微信，h5 嵌入 app 应用等多终端
  - 配置微信/企业微信平台接口权限
  - 部署至 Nginx 与 IIS，确保多终端运行稳定
- **成果**：
  - 负责整个改单的建立、开发、h5的改单使用nginx部署项目，小程序的需审核发布，企业微信的项目使用测试企业微信部署测试，以及后期的维护
  - 独立完成页面开发与数据接口对接
  - 项目跨终端正常运行，支付与考勤模块稳定性提升
  - 系统部署效率提升

#### **PC 端内部管理平台**

- **技术栈**：Vue、Angular、qiankun 微前端框架、Node.js、Element UI
- **功能模块**：
  - DAS-MP 内部管理平台（高级管理：标签定义、分类定义、词汇定义、基本产品、产品包分类管理、序列管理、短信渠道、默认关闭管理、数据导出、前端资源传输到环境，一级菜单：模块功能点、菜单管理、URL定义、接口返回定义、前端资源、设备类型定义、数据选项管理、参数定义、数据实体、数据模版、消息定义，租户及授权：销售产品包管理、授权基本项定义、租户管理、C3-Plus授权、融合C3-Plus和AIoT授权），登录、修改密码、用户管理、角色管理、环境配置管理、个人设置）
  - C3-Plus OM 运管平台（模块功能点、菜单管理、URL功能点、APICodeMsg、服务管理、产品数据更新、使用许可证、功能关闭与黑白名单管理，登录），
  - C3-Plus SaaS 系统维护（租户管理、租户数据库升级、用户管理、设备列表、解析器配置，登录）
  - C3-Plus 运管平台（消费、考勤、门禁监控、设备服务（设备列表、解析器配置、设备权限下载查询、FTP下载任务查询、设备权限差异汇总、设备权限差异详情、派梯楼层查询、设备运行日志、服务集群、设备接入，系统配置）、主应用等子系统）
- **性能优化**：
  - 微前端架构优化，支持多系统无缝集成
  - 图表数据懒加载，减少渲染压力
- **部署与适配**：
  - 跨浏览器兼容（Chrome、IE11）
  - 模块按需加载，降低首屏压力
- **成果**：
  - 构建DAS-MP 内部管理平台，C3-Plus OM 运管平台和C3-Plus SaaS 系统维护，负责主要开发和维护
  - C3-Plus 运管平台参与消费、考勤、门禁监控、设备服务、主应用等子系统开发
  - 提升多系统协作效率
  - 前端性能提升

#### **移动端 C3-Plus 应用**

- **技术栈**：uniapp、uView UI 框架、Vuex、Axios、ES6、Promise
- **功能模块**：
  - 登录/注册（微信授权登录、Token 鉴权）
  - 主页（信息总览、快捷入口）
  - 我的（个人信息管理、消息通知）
- **性能优化**：
  - 使用 Vuex 管理全局状态，减少数据冗余
  - 图片资源按需加载，首页加载速度提升
  - 接口请求统一封装，支持错误处理与重试机制
- **部署与适配**：
  - 适配 iOS / Android 与微信小程序
  - 按需引入组件，减少包体积
  - Flex 布局与媒体查询实现多屏幕适配
- **成果**：
  - 提升移动端用户操作流畅度
  - 统一 UI 风格，降低维护成本



### 中联智科高新技术有限公司 | 研发部-前端开发

_2020.03 - 2021.03_

#### **智慧鱼池**

- **技术栈**：Angular、Ionic（移动端）、Vue、Avue、Element UI、OpenLayers 地图
- **功能模块**：实时监控鱼池环境，日志管理与多租户支持，养鱼设备管理与消息通知，ERP 系统集成
- **性能优化**：地图数据分层加载，表格分页与虚拟滚动优化
- **部署与适配**：移动端与 PC 端双平台适配，组件按需加载，减少资源占用
- **成果**：建立移动端模型和PC端的构建，移动端与 PC 端功能稳定运行，数据可视化响应速度提升

#### **智慧畜牧**

- **技术栈**：Angular、Ionic（移动端）、Vue、Avue、Element UI、OpenLayers 地图
- **功能模块**：牲畜实时监控，颈圈佩戴与电子围栏管理，地图定位与轨迹回放，牛脸识别与日志管理，消息管理，入栏数数和设备管理。
- **性能优化**：实时定位数据压缩传输，图片识别接口并发优化
- **部署与适配**：移动端与 PC 双平台支持
- **成果**：实现散养与圈养模式切换，数据传输稳定性提升，实现摄像头，颈圈的安装，拆卸，转换，查看。牲畜管理，牲畜跨越电子围栏实时定位牲畜位置。



### 析研科技有限公司（实习） | 开发部-前端开发工程师

_2019.10 - 2020.01_

#### **香港补习教育系统**

- 技术栈：Vue、Spring Boot、Element UI、WebSocket
- 项目描述：该项目属于线上补习教育类，基于多语言处理的前后端分离开发，支持Web端和手机端线上服务。有学生端和教师端。上堂以实时直播的形式实现，根据个人信息级、筛选条件和课程安排选择配对。
- 功能模块：时间表管理、课程配对、我的，笔记管理与上堂等。
- 性能优化：WebSocket 保持低延迟实时通信，界面组件按需加载
- 部署与适配：跨浏览器兼容
- 成果：完成核心交互逻辑与数据渲染，提升直播课程稳定性。对接数据接口，封装函数处理数据库中的json数据，展示到页面。根据UI 设计的项目设计稿，使用Element中的组件，编写代码实现页面展示。测试前端功能和前端与后端数据接口的实现。



## 项目经验（毕业设计）

**图片分享网站 | 独立开发**

- 类似 Instagram，支持注册登录、上传图片、评论、点赞
- 七牛云图片存储与处理，腾讯云服务器，百度云数据库
- 图片懒加载、MD5+Salt 密码加密、响应式图片缩放
- 项目地址：https://github.com/changxiangyu666/PycharmProjects



## 荣誉奖项

- 优秀学生二等奖学金
- 优秀学生三等奖学金

![毕业证书](images/credential/biyezhengshu.jpg)
![学位证书](images/credential/xueweizhengshu.jpg)
</div>

<style>
/* ==========================================================================
   1. 全局变量定义 (方便统一调整)
   ========================================================================== */
:root {
    /* 核心配色 */
    --primary-color: #007bff;      /* 主色调：蓝色 */
    --text-main: #2c3e50;          /* 主要文字颜色 */
    --text-sub: #666666;           /* 次要文字颜色 */
    --bg-header: #f8f9fa;          /* 标题背景色 */
    
    /* 间距与字体 */
    --spacing-base: 20px;
    --font-base: 16px;             /* 网页浏览字体大小 */
    --line-height: 1.6;
}

/* 简历容器 - 模拟 A4 纸质感或干净的网页布局 */
.resume-wrapper {
    max-width: 850px;
    margin: 40px auto;
    padding: 40px;
    background: #fff;
    color: #333;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    line-height: var(--line-height);
    border-radius: 8px;
}

/* ---------------- 头部信息 ---------------- */
.resume-wrapper h1 {
    font-size: 2.5rem;
    color: var(--text-main);
    border-bottom: none;
    margin-bottom: 0.5rem;
    text-align: center;
    font-weight: 700;
}

/* 基本信息列表转横排 */
.resume-wrapper h1 + ul {
    list-style: none;
    padding: 0;
    text-align: center;
    margin-bottom: 30px;
    font-size: 0.95rem;
    color: var(--text-sub);
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 15px;
}

.resume-wrapper h1 + ul li {
    display: inline-block;
}

/* 链接样式 */
.resume-wrapper a {
    color: var(--primary-color);
    text-decoration: none;
    border-bottom: 1px dashed var(--primary-color);
    transition: all 0.2s;
}
.resume-wrapper a:hover {
    color: #0056b3;
    border-bottom-style: solid;
}

/* ---------------- 标题通用样式 ---------------- */
.resume-wrapper h2 {
    font-size: 1.8rem;
    color: var(--text-main);
    border-bottom: 2px solid #eaeaea;
    padding-bottom: 10px;
    margin-top: 40px;
    margin-bottom: 20px;
    position: relative;
}

.resume-wrapper h2::after {
    content: '';
    position: absolute;
    bottom: -2px;
    left: 0;
    width: 60px;
    height: 2px;
    background-color: #007bff; /* 强调色条 */
}

/* ---------------- 核心技能 ---------------- */
/* 针对技能列表的样式优化 */
.resume-wrapper h2 + ul li {
    margin-bottom: 8px;
}
.resume-wrapper h2 + ul li strong {
    color: var(--text-main);
    font-weight: 600;
    display: inline-block;
    min-width: 100px; /* 让技能分类对齐 */
}

/* ---------------- 工作经历 ---------------- */
/* 建议 HTML 结构：
   <h3>
     <span>公司名称</span>
     <span class="job-title">职位名称</span>
     <span class="job-date">2020.01 - 至今</span>
   </h3>
   如果无法修改HTML，以下CSS依然兼容您的 h3 + p 结构
*/
.resume-wrapper h3 {
    font-size: 1.6rem;
    color: #333;
    margin-top: 25px;
    margin-bottom: 15px;
    background-color: var(--bg-header);
    padding: 10px 15px;
    border-left: 4px solid var(--primary-color);
    border-radius: 0 4px 4px 0;

    /* Flex 布局实现两端对齐 */
    display: flex; 
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
}
/* 针对原结构的补丁：如果时间写在 h3 外面的 p 标签里 */
.resume-wrapper h3 + p {
    margin: -38px 15px 15px 0; /* 负边距拉上去 */
    text-align: right;
    pointer-events: none; /* 防止遮挡点击 */
}
/* 处理时间段 */
.resume-wrapper h3 + p em {
    display: block;
    margin-bottom: 15px;
    font-style: normal;
    color: var(--text-sub);
    font-size: 1.4rem;
    text-align: right;
    margin-top: -40px; /* 调整位置与标题同行或紧随其后 */
    padding-right: 15px;
    background: var(--bg-header); /* 遮挡背景防止文字重叠 */
}

/* 项目名称 */
.resume-wrapper h4 {
    font-size: 1.5rem;
    color: #444;
    margin-top: 20px;
    margin-bottom: 10px;
    font-weight: 600;
}

/* 项目详情列表 */
.resume-wrapper ul {
    padding-left: 20px;
}

.resume-wrapper li {
    margin-bottom: 6px;
    color: #444;
}

/* ---------------- 图片展示 ---------------- */
.resume-wrapper img {
    max-width: 100%;
    height: auto;
    margin: 10px;
    border: 1px solid #ddd;
    padding: 5px;
    border-radius: 4px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

/* ==========================================================================
   4. 打印专用样式 (Ctrl+P 触发)
   ========================================================================== */
@media print {
    @page {
        margin: 1cm; /* 标准 A4 边距 */
        size: A4 portrait;
    }

    body {
        background: #fff;
        -webkit-print-color-adjust: exact; /* 强制打印背景色 (Chrome/Safari) */
        print-color-adjust: exact;         /* 强制打印背景色 (Firefox) */
    }

    .resume-wrapper {
        width: 100%;
        max-width: none;
        margin: 0;
        padding: 0;
        box-shadow: none;
        border: none;
    }

    /* 字体调整：使用 pt 单位更适合打印 */
    .resume-wrapper { font-size: 10.5pt; line-height: 1.4; }
    .resume-wrapper h1 { font-size: 18pt; margin-bottom: 5pt; }
    .resume-wrapper h1 + ul { font-size: 9pt; margin-bottom: 15pt; gap: 10pt; }
    .resume-wrapper h2 { font-size: 14pt; margin-top: 15pt; margin-bottom: 8pt; padding-bottom: 3pt; }
    .resume-wrapper h3 { font-size: 11pt; margin-top: 10pt; padding: 4pt 8pt; }
    .resume-wrapper h4 { font-size: 11pt; margin-top: 8pt; }
    
    /* 隐藏网页端特有的元素 */
    .resume-wrapper a { border-bottom: none; color: #000; text-decoration: none; }

    /* 关键：分页控制 */
    h2, h3, h4 { page-break-after: avoid; } /* 标题后不分页 */
    li { page-break-inside: avoid; }        /* 列表项内部不分页 */
    section, div { page-break-inside: auto; }

    /* 打印时的布局微调 */
    .resume-wrapper h3 + p {
        margin-top: -32px; /* 打印时微调位置 */
    }
    
    /* 移除不必要的装饰 */
    .resume-wrapper img { display: none; } /* 简历通常不打印装饰图片，除非是作品集 */
    .sidebar,.right-sidebar,.left-sidebar,.sticky,.article-details,.site-footer,.disqus-container,.article-footer { display: none; }
    
}

/* ==========================================================================
   5. 移动端适配
   ========================================================================== */
@media (max-width: 600px) {
    .resume-wrapper {
        padding: 20px;
        margin: 0;
        border-radius: 0;
    }
    
    /* 移动端取消负边距，改为流式布局 */
    .resume-wrapper h3 {
        flex-direction: column;
        align-items: flex-start;
    }
    
    .resume-wrapper h3 + p {
        margin: 0 0 10px 0;
        text-align: left;
    }
    
    .resume-wrapper h3 + p em {
        background: none;
        padding-left: 0;
        display: block;
        margin-bottom: 5px;
        color: #888;
        font-size: 0.85rem;
    }
}
</style>


