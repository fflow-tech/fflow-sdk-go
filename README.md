# 🚀 fflow-sdk-go

<div align="center">
  <img src="https://img.shields.io/badge/Go-1.18+-00ADD8?style=flat-square&logo=go&logoColor=white" alt="Go Version">
  <img src="https://img.shields.io/badge/Status-Active-success?style=flat-square" alt="Status">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=flat-square" alt="License">
</div>

English | [简体中文](./README_zh.md)

## 📖 Introduction

`fflow-sdk-go` is a Go SDK for fflow, providing a comprehensive development framework for FaaS (Function as a Service). This SDK enables developers to easily build, deploy, and manage serverless functions without worrying about the complexity of underlying infrastructure.

## ✨ Features

- 🛠️ Simple function development interface
- 🔄 Complete context management system
- 📝 Integrated logging functionality
- 💾 Built-in storage interface
- 📊 Metadata management support
- 🔌 Flexible extension capabilities

## 🔧 Installation

```bash
go get github.com/fflow-tech/fflow-sdk-go
```

Ensure your project uses Go 1.23 or higher.

## 🚀 Quick Start

Here's a simple example of creating a function using fflow-sdk-go:

```go
package main

import (
    "github.com/fflow-tech/fflow-sdk-go/faas"
)

// Define your function handler
func MyFunction(ctx faas.Context, event map[string]interface{}) (interface{}, error) {
    // Get logger
    logger := ctx.Logger()
    logger.Infof("Function started, processing event: %v", event)
    
    // Use storage functionality
    storage := ctx.Storage()
    err := storage.Set("lastExecution", "success", 3600) // Valid for 1 hour
    if err != nil {
        logger.Errorf("Storage operation failed: %v", err)
        return nil, err
    }
    
    // Get metadata
    metadata := ctx.Metadata()
    functionName := metadata.Name()
    functionVersion := metadata.Version()
    logger.Infof("Current function: %s, version: %d", functionName, functionVersion)
    
    // Return result
    return map[string]string{
        "status": "success",
        "message": "Function executed successfully",
    }, nil
}

func main() {
    // Function registration and startup logic
    // ...
}
```

## 📚 Core Interfaces

### Context

`Context` is the function execution context interface, providing:

- `Logger()` - Returns logging interface
- `Logs()` - Gets all recorded logs
- `Storage()` - Returns storage interface
- `Metadata()` - Returns metadata interface
- `Context()` - Returns Go standard library's context.Context

### Logger

`Logger` provides logging functionality:

- `Infof()` - Log info level messages
- `Warnf()` - Log warning level messages
- `Debugf()` - Log debug level messages
- `Errorf()` - Log error level messages

### Storage

`Storage` provides data storage functionality:

- `Get()` - Retrieve stored data
- `Set()` - Set data with expiration time
- `Del()` - Delete stored data

### Metadata

`Metadata` provides function metadata management:

- `Namespace()` - Get function namespace
- `ID()` - Get function ID
- `Name()` - Get function name
- `Version()` - Get function version
- `Attribute()` - Get specified function attribute

## 💻 Advanced Example

### HTTP Trigger Function

```go
package main

import (
    "encoding/json"
    "github.com/fflow-tech/fflow-sdk-go/faas"
)

func HttpHandler(ctx faas.Context, event map[string]interface{}) (interface{}, error) {
    logger := ctx.Logger()
    
    // Parse HTTP request
    request, ok := event["request"].(map[string]interface{})
    if !ok {
        logger.Errorf("Invalid request format")
        return map[string]interface{}{
            "statusCode": 400,
            "body": "Invalid request format",
        }, nil
    }
    
    // Process request
    method, _ := request["method"].(string)
    path, _ := request["path"].(string)
    logger.Infof("Received %s request: %s", method, path)
    
    // Return HTTP response
    return map[string]interface{}{
        "statusCode": 200,
        "headers": map[string]string{
            "Content-Type": "application/json",
        },
        "body": json.Marshal(map[string]string{
            "message": "Request processed successfully",
            "path": path,
            "method": method,
        }),
    }, nil
}
```

## 🔍 Use Cases

- **Microservices**: Build lightweight, scalable microservices
- **API Backend**: Rapidly develop API backend services
- **Data Processing**: Event-triggered data processing tasks
- **Scheduled Tasks**: Periodically executed scheduled tasks
- **Event Response**: Event-driven processing logic

## 🤝 Contributing

We welcome all forms of contributions, whether they're new features, documentation improvements, or bug reports. Please follow these steps to submit your contribution:

1. Fork this repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
