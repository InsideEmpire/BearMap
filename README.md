# BearMap

BearMap 是一个基于 Java 的地图导航应用，提供地图渲染、路径规划和导航指令等功能。该项目使用 OpenStreetMap 数据构建地图，并通过 Web 界面呈现给用户。

## 项目结构

```
.
├── data                    # 数据文件夹
│   ├── berkeley-2018.osm.xml  # OSM 地图数据
│   ├── proj3_imgs             # 地图瓦片图像
│   └── words.txt              # 文本数据
├── src                     # 源代码
│   └── main
│       └── java            # Java 源文件
│           ├── example     # 示例代码
│           │   ├── CSCourseDB.java           # 课程数据库示例
│           │   └── CSCourseDBHandler.java    # XML解析处理器示例
│           ├── GraphBuildingHandler.java     # OSM XML 数据解析器
│           ├── GraphDB.java                  # 图数据库实现
│           ├── MapServer.java                # Web 服务器实现
│           ├── Rasterer.java                 # 地图瓦片渲染器
│           └── Router.java                   # 路径规划算法实现
├── pom.xml                 # Maven 配置文件
└── README.md               # 本文档
```

## 核心功能

### 1. 地图渲染 (Rasterer)

将地图划分为不同级别的瓦片，根据用户请求的视图范围和大小，选择合适的瓦片进行渲染。`Rasterer` 类管理地图的层级结构和瓦片选择逻辑。

主要特点：
- 支持多级缩放（0-7级）
- 根据查询边界和请求尺寸计算最优图块
- 使用经纬度每像素距离(LonDPP)确定合适的缩放级别

### 2. 图数据结构 (GraphDB)

从 OSM XML 文件中构建地图图数据结构，存储节点(Node)和边(Edge)信息。

主要组件：
- `Node` 类：表示地图上的地点，包含经纬度和相邻节点信息
- `Edge` 类：表示地图上的道路，连接不同的节点
- 使用 `GraphBuildingHandler` 解析 OSM XML 数据

### 3. 路径规划 (Router)

使用 A* 算法实现最短路径查找，提供从起点到终点的最优路线。

主要功能：
- `shortestPath` 方法查找两点间最短路径
- `routeDirections` 方法生成导航指令
- 支持多种导航指示：直行、左转、右转、轻微左转、轻微右转等

### 4. Web 服务 (MapServer)

提供 Web API 接口，处理前端的地图渲染、路径规划等请求。

主要端点：
- `/raster` - 获取地图瓦片渲染
- `/route` - 计算两点间的最短路径
- `/search` - 搜索地点
- `/clear_route` - 清除当前路径

## 技术栈

- Java 8
- Maven 构建工具
- Spark Java Web 框架 (v2.7.2)
- GSON JSON 解析库 (v2.8.2)
- XML SAX 解析器
- JUnit 测试框架 (v4.12)
- SLF4J 日志框架 (v1.7.25)

## 如何运行

运行 MapServer.java 主类

在浏览器中访问
   ```
   http://localhost:4567/
   ```

## 开发者指南

### 地图数据解析

项目使用 SAX 解析器处理 OSM XML 数据。`GraphBuildingHandler` 类继承自 `DefaultHandler`，在解析过程中构建图数据结构。

### 地图渲染原理

地图渲染基于瓦片系统，每个瓦片大小为 256x256 像素。不同缩放级别的瓦片覆盖不同大小的地理区域。`Rasterer` 类负责根据用户请求选择合适的瓦片集合。

### 路径规划算法

路径规划使用 A* 算法，这是 Dijkstra 算法的一个改进版本，使用启发式函数估计从当前节点到目标的距离。在本项目中，使用了地理距离作为启发式函数。

### 导航指令生成

根据路径中的转弯角度，生成人类可读的导航指令，如"左转"、"直行"等。角度计算基于球面几何。

## 示例代码

项目包含一个 `example` 包，其中有 `CSCourseDB` 和 `CSCourseDBHandler` 类，演示了如何使用 SAX 解析器处理 XML 数据。这是一个简化的示例，帮助理解 `GraphBuildingHandler` 的工作原理。

## 开发者信息

本项目是基于 CS61B 课程的 Proj3 项目实现，使用 Berkeley 的 BearMaps 骨架代码 v2.0 为基础。它展示了数据结构、算法、Web 服务和地理信息系统的综合应用。
