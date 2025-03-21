# 🚀 fflow-sdk-go

<div align="center">
  <img src="https://img.shields.io/badge/Go-1.18+-00ADD8?style=flat-square&logo=go&logoColor=white" alt="Go Version">
  <img src="https://img.shields.io/badge/Status-Active-success?style=flat-square" alt="Status">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=flat-square" alt="License">
</div>

## 📖 简介

`fflow-sdk-go` 是一个功能强大的 Go 语言 SDK，为 FaaS（函数即服务）提供完整的开发框架。该 SDK 使开发者能够轻松构建、部署和管理无服务器函数，无需关心底层基础设施的复杂性。

## ✨ 特性

- 🛠️ 简洁的函数开发接口
- 🔄 完整的上下文管理系统
- 📝 集成的日志管理功能
- 💾 内置的存储接口
- 📊 元数据管理支持
- 🔌 灵活的扩展能力

## 🔧 安装

```bash
go get github.com/fflow-tech/fflow-sdk-go
```

确保您的项目使用 Go 1.18 或更高版本。

## 🚀 快速开始

以下是使用 fflow-sdk-go 创建函数的简单示例：

```go
package main

import (
    "github.com/fflow-tech/fflow-sdk-go/faas"
)

// 定义您的函数处理器
func MyFunction(ctx faas.Context, event map[string]interface{}) (interface{}, error) {
    // 获取日志记录器
    logger := ctx.Logger()
    logger.Infof("函数已启动，处理事件: %v", event)
    
    // 使用存储功能
    storage := ctx.Storage()
    err := storage.Set("lastExecution", "success", 3600) // 有效期1小时
    if err != nil {
        logger.Errorf("存储操作失败: %v", err)
        return nil, err
    }
    
    // 获取元数据
    metadata := ctx.Metadata()
    functionName := metadata.Name()
    functionVersion := metadata.Version()
    logger.Infof("当前执行函数: %s, 版本: %d", functionName, functionVersion)
    
    // 返回结果
    return map[string]string{
        "status": "success",
        "message": "函数执行成功",
    }, nil
}

func main() {
    // 函数注册和启动逻辑
    // ...
}
```

## 📚 主要接口

### Context

`Context` 是函数执行的上下文接口，提供以下功能：

- `Logger()` - 返回日志记录接口
- `Logs()` - 获取所有记录的日志
- `Storage()` - 返回存储接口
- `Metadata()` - 返回元数据接口
- `Context()` - 返回 Go 标准库的 context.Context

### Logger

`Logger` 提供日志记录功能：

- `Infof()` - 记录信息级别日志
- `Warnf()` - 记录警告级别日志
- `Debugf()` - 记录调试级别日志
- `Errorf()` - 记录错误级别日志

### Storage

`Storage` 提供数据存储功能：

- `Get()` - 获取存储的数据
- `Set()` - 设置数据并指定过期时间
- `Del()` - 删除存储的数据

### Metadata

`Metadata` 提供函数元数据管理：

- `Namespace()` - 获取函数命名空间
- `ID()` - 获取函数 ID
- `Name()` - 获取函数名称
- `Version()` - 获取函数版本
- `Attribute()` - 获取指定的函数属性

## 💻 进阶示例

### HTTP 触发器函数

```go
package main

import (
    "encoding/json"
    "github.com/fflow-tech/fflow-sdk-go/faas"
)

func HttpHandler(ctx faas.Context, event map[string]interface{}) (interface{}, error) {
    logger := ctx.Logger()
    
    // 解析 HTTP 请求
    request, ok := event["request"].(map[string]interface{})
    if !ok {
        logger.Errorf("无效的请求格式")
        return map[string]interface{}{
            "statusCode": 400,
            "body": "无效的请求格式",
        }, nil
    }
    
    // 处理请求
    method, _ := request["method"].(string)
    path, _ := request["path"].(string)
    logger.Infof("收到 %s 请求: %s", method, path)
    
    // 返回 HTTP 响应
    return map[string]interface{}{
        "statusCode": 200,
        "headers": map[string]string{
            "Content-Type": "application/json",
        },
        "body": json.Marshal(map[string]string{
            "message": "请求处理成功",
            "path": path,
            "method": method,
        }),
    }, nil
}
```

## 🔍 使用场景

- **微服务**: 构建轻量级、可扩展的微服务
- **API 后端**: 快速开发 API 后端服务
- **数据处理**: 触发式数据处理任务
- **定时任务**: 周期性执行的定时任务
- **事件响应**: 基于事件触发的处理逻辑

## 🤝 贡献指南

我们欢迎所有形式的贡献，无论是新功能、文档改进还是问题报告。请遵循以下步骤提交您的贡献：

1. Fork 本仓库
2. 创建您的特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交您的更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 打开一个 Pull Request

## 📄 许可证

本项目采用 [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0) 许可证。
