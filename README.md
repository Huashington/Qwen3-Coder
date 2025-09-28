# 股票信息展示网站项目分析

## 1. 项目背景

这是一个基于JavaScript的全栈股票信息展示网站，采用现代前后端分离架构：
- **前端**：使用React框架构建用户界面
- **后端**：采用Node.js+Express提供API服务
- **架构特点**：
  - 清晰的模块划分（client/server）
  - 独立的依赖管理（两个package.json）
  - 开发环境与生产环境分离


### 核心文件说明

1. **前端核心文件**：
   - `App.js`：React主组件
   - `StockList.js`：股票列表展示组件
   - `StockSearch.js`：股票搜索组件
   - `index.js`：前端入口文件

2. **后端核心文件**：
   - `index.js`：Express服务入口文件