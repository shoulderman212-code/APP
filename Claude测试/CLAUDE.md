# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

社区会员系统产品设计与原型验证工作区。主要产出为基于 PRD 的静态 HTML 原型页面，用于需求评审和设计验证。

## 关键路径

| 用途 | 路径 |
|------|------|
| PRD 文档 | `/Users/guohaoqiu/Documents/需求文档/PRD/` |
| 需求分析文档 | `docs/requirements/` |
| 原型文件 | 当前目录及 `prototypes/` |
| 样式规范 | `/Users/guohaoqiu/Downloads/STYLE_GUIDE.md` |
| 自定义技能 | `~/.agents/skills/` |
| 设计质检验证 | 参考 `design-qa.md` |

## 当前原型

**主文件**: `社区活动奖品层原型.html` — 社区活动奖品层 V1.0 完整原型

### 页面结构（左侧导航）

运营后台：
- 活动奖品配置 — 列表 + 新增/编辑弹窗（三种奖品类型动态表单）
- 奖励记录管理 — 多维筛选 + 列表 + 详情弹窗（状态时间轴）
- 异常处理 — 统计卡片 + 异常列表 + 自动重试/人工补发
- 实物履约管理 — 履约列表 + 发货状态流转
- 兑换码明细 — 统计卡片 + 筛选 + 码列表 + 导入弹窗（CSV 表格导入）

用户端：
- 我的奖励 — 不同奖品类型卡片（软件码/优惠券/实物）
- 软件码详情 — 查看/复制
- 优惠券详情 — 直接到账/领取资格
- 实物领取 — 收货信息填写

### 统一状态体系

发放详情和兑换码明细共用三个状态：**未发放**（灰）/ **已发放**（蓝）/ **已使用**（绿）

### 新增奖品弹窗

选择奖品类型后动态切换配置区：
- 虚拟商品：选码池 → 下载模版 → 上传 CSV → 预览 → 确认导入
- 优惠券：输入券模板ID → 同步券信息 → 选发放模式
- 实物商品：下拉选商城商品 → 展示商品详情

## 原型开发规范

- 纯 HTML/CSS/JS 单文件，Tailwind CSS CDN
- 严格遵循 STYLE_GUIDE.md：主色 `#1677ff`，背景 `#f5f5f5`，卡片 `rounded-2xl`，状态章 `rounded-full text-[10px]`，弹窗 `backdrop-blur-sm`
- 所有界面文案使用中文
- 弹窗用 `modal-overlay` + `showModal`/`hideModal`，页面切换用 `switchPage`
- Toast 通知用 `showToast(msg, type)`，type 为 `success` 或 `error`

## 截图文件夹

与 PRD 文件同目录，命名规则：`{PRD名称去掉.md和末尾PRD}screen shot/`

## 本地预览

```bash
open 社区活动奖品层原型.html
# 或
npx serve . -p 4173
```

## 工作流

1. 需求分析文档 → PRD 生成（可用 `prd-generator` 技能）
2. PRD + 样式规范 → HTML 原型
3. 浏览器预览 → 截图反馈 → 迭代修改
4. 设计 QA 验证（参考 `design-qa.md`）
