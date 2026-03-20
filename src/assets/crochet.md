产品需求文档 (PRD) - “织心 (YarnHub)”
项目名称	织心 (YarnHub)
产品目标	打造国内首款针对钩/棒针爱好者的极简、沉浸式编织助手
核心定位	灵感记录 + 智能计数 + 线材管理（PWA 离线优先）
开发周期	8 周 (MVP 上线)


1. 产品概述
“织心”旨在解决国内编织爱好者在看图解时“记不住行数”、“针号换算难”、“线材库存乱”的痛点。通过 PWA 技术提供类原生 App 的体验，无需下载，扫码即用。

1. 核心功能模块
2.1 沉浸式工具箱 (Tools)

多任务切换计数器：支持在一个项目下开启多个子任务（如：左袖、右袖、领口）。

Swipe 手势交互：支持在不同任务卡片间左右滑动切换，操作丝滑。

国内针号对照系统：

钩针：支持 2/0 号、3/0 号等国内标号与公制 (mm) 互转。

棒针：支持国内 6-14 号与公制 (mm) 的反向换算（数字越大针越细）。

生产力特性：支持 Web Wake Lock API（屏幕常亮），防止编织时手机黑屏。

2.2 智能图解助手 (Pattern Assistant)

简写解析器 (Pattern Parser)：用户输入 R1: 6X; R2: 6V，系统自动解析并渲染为可点击的打卡任务列表。

PDF/图片指引条：支持上传本地图解图片，提供一个可拖动的半透明水平辅助线。

计时器：记录每个项目的纯编织时长，支持暂停与自动保存。

2.3 线材图书馆 (Yarn Stash)

国内品牌预设：内置苏苏姐家、恒源祥、小辛娜娜、编织人生等品牌及其热门型号。

库存联动：项目完成后，系统提示扣减对应线材存量（团数/克数）。

3. 技术路线图 (Tech Stack)
前端 (Frontend)

框架：Vue 3 (Composition API) + TypeScript

状态管理：Pinia + 持久化插件

UI 组件：Vant UI (移动端适配)

分发：PWA (Progressive Web App)

后端 (Backend)

语言框架：Java + Spring MVC (Spring Boot 3.x)

持久层：MyBatis-Plus

数据库：MySQL 8.0

认证：JWT (JSON Web Token)

4. 核心数据模型 (Data Schema)
4.1 项目表 (t_project)

字段	类型	说明
id	BigInt	主键
title	Varchar	项目名称
category	TinyInt	1-钩针, 2-棒针
needle_size	Varchar	存储如 "5/0 (3.0mm)"
yarn_json	Text	存储关联线材的 JSON 快照
total_seconds	BigInt	累计编织时长
4.2 任务列表表 (t_project_task)

字段	类型	说明
id	BigInt	主键
project_id	BigInt	关联项目 ID
row_content	Varchar	解析后的指令内容
is_done	Boolean	是否已打卡完成
5. 开发里程碑 (Milestones)
W1-W2 (基础构建)：后端 Spring Boot 环境搭建，MySQL 表设计，前端 Vue 3 基础框架。

W3-W4 (核心逻辑)：实现“简写解析器”正则逻辑，开发单任务计数器与本地存储逻辑。

W5-W6 (多端同步)：开发 Java 同步接口，支持多任务 Swipe 切换，实现国内针号转换器。

W7-W8 (优化上线)：线材预设库数据录入，UI 细节微调（莫兰迪配色），部署至云服务器并开启 PWA 支持。

6. 未来规划 (V2.0)
语音控制：通过语音指令（“下一行”）自动增加计数。

社交分享：生成精美的项目长图分享至小红书。

AR 测距：利用摄像头初步测量织物尺寸。