# 股票信息分析网站
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9e78e2cc-2cd7-49ac-b922-2bc0221bcd79" />


## 1. 前端核心功能实现 (`client/src/`)

### 1.1 主组件架构 (`App.js`)

```javascript
import React from 'react';
import StockList from './StockList';
import StockSearch from './StockSearch';

function App() {
  return (
    <div className="app-container">
      <h1>股票信息展示平台</h1>
      <StockSearch />
      <StockList />
    </div>
  );
}

export default App;
```

**功能说明**：
- 集成两个核心子组件：`StockSearch`和`StockList`
- 提供基础页面布局和标题
- 作为前端路由的根组件（虽然当前未显示路由配置）

### 1.2 股票搜索组件 (`StockSearch.js`)

```javascript
import React, { useState } from 'react';

function StockSearch() {
  const [searchTerm, setSearchTerm] = useState('');
  
  const handleSearch = (e) => {
    e.preventDefault();
    
    console.log('搜索股票:', searchTerm);
  };

  return (
    <div className="search-container">
      <form onSubmit={handleSearch}>
        <input
          type="text"
          placeholder="输入股票代码"
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
        />
        <button type="submit">搜索</button>
      </form>
    </div>
  );
}

export default StockSearch;
```

**功能说明**：
- 实现受控表单组件
- 使用React Hooks管理搜索词状态
- 提供基本的表单提交处理（当前仅打印到控制台）

### 1.3 股票列表组件 (`StockList.js`)

```javascript
import React, { useEffect, useState } from 'react';

function StockList() {
  const [stocks, setStocks] = useState([]);
  
  useEffect(() => {
    
    fetch('http://localhost:3001/api/stocks')
      .then(response => response.json())
      .then(data => setStocks(data))
      .catch(error => console.error('获取股票数据失败:', error));
  }, []);

  return (
    <div className="stock-list">
      <h2>股票列表</h2>
      <ul>
        {stocks.map(stock => (
          <li key={stock.symbol}>
            {stock.symbol}: {stock.name} - 价格: {stock.price}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default StockList;
```

**功能说明**：
- 使用`useEffect`实现组件挂载时的数据获取
- 通过`fetch`调用后端API
- 渲染股票列表（当前为模拟数据）
- 包含基本的错误处理

## 2. 后端核心功能实现 (`server/index.js`)

```javascript
const express = require('express');
const app = express();
const PORT = 3001;

// 模拟股票数据
const mockStocks = [
  { symbol: 'AAPL', name: 'Apple Inc.', price: 189.30 },
  { symbol: 'MSFT', name: 'Microsoft Corp.', price: 420.72 },
  { symbol: 'GOOGL', name: 'Alphabet Inc.', price: 171.25 }
];

// 中间件
app.use(express.json());

// API路由
app.get('/api/stocks', (req, res) => {
  res.json(mockStocks);
});

// 启动服务器
app.listen(PORT, () => {
  console.log(`服务器运行在 http://localhost:${PORT}`);
});
```

**功能说明**：
- 创建Express应用实例
- 定义模拟股票数据
- 设置JSON中间件
- 提供`/api/stocks`GET端点返回股票数据
- 在3001端口启动服务器

## 3. 前后端交互流程

1. **前端初始化**：
   - `StockList`组件挂载时调用`fetch`
   - 请求`http://localhost:3001/api/stocks`

2. **后端处理**：
   - Express接收GET请求
   - 返回模拟的股票数据数组

3. **前端渲染**：
   - 将获取的数据映射为列表项
   - 显示股票代码、名称和价格
