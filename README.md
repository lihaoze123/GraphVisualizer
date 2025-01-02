# GraphVisualizer

一个针对图论的可视化工具，基于 Vue 3 开发。支持有向图和无向图的绘制、编辑与导出，是学习图论算法和展示图形结构的理想工具。

## 功能特性

- 💫 实时图形渲染
- 🔄 支持有向图和无向图切换
- 💾 图形导出功能
- 🎨 美观的用户界面
- 📱 响应式设计

## 技术栈

- Vue 3
- v-network-graph (图形渲染库)
- Dagre (图形布局算法)

## 安装

```bash
# 克隆项目
git clone https://github.com/lihaoze/GraphVisualizer.git

# 进入项目目录
cd GraphVisualizer

# 安装依赖
npm install

# 启动开发服务器
npm run dev
```

## 使用方法

1. 在左侧输入面板中输入边的信息，格式为：
   ```
   1 2
   2 3
   3 1
   ```
   每行表示一条边，两个数字之间用空格分隔，分别表示起点和终点。

2. 选择图形类型（有向图/无向图）

3. 图形会自动渲染在右侧面板中

4. 使用导出功能保存图形

## 许可证

MIT License
