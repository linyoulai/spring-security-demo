# Spring Boot 安全认证入门示例

这是一个使用 **Spring Boot 3.2.0** 和 **Spring Security 6** 构建的基础 Web 安全项目。

本项目的核心目标是演示如何快速搭建一个包含**身份认证**和**受保护 API** 的应用程序，是理解 Spring Security 工作原理的绝佳入门案例。

## 功能特性

*   **RESTful API:**
    *   一个**受保护**的 `GET /api/hello` 接口，只有在认证后才能访问。
*   **身份认证:**
    *   内置了一个基于**表单登录**的认证流程，自动提供了 `POST /login` 登录接口。
    *   使用**内存用户存储**，预设了一个测试用户，无需数据库。
        *   **用户名:** `test`
        *   **密码:** `123456`
*   **安全性:**
    *   使用 **BCrypt** 对密码进行强哈希加盐存储。

## 技术栈

*   **核心框架:** Spring Boot 3.2.0
*   **安全框架:** Spring Security 6
*   **Web 框架:** Spring Web (内置 Tomcat)
*   **构建工具:** Maven
*   **开发语言:** Java 17

## 如何在本地运行

### 环境要求

*   JDK 17 或更高版本
*   Apache Maven 3.6+

### 运行步骤

1.  **克隆项目到本地:**
    ```bash
    git clone https://github.com/你的用户名/security-demo.git
    cd security-demo
    ```

2.  **使用 Maven 构建项目:**
    ```bash
    mvn clean package
    ```
    这会在 `target/` 目录下生成一个可执行的 `.jar` 文件。

3.  **运行 Spring Boot 应用:**
    ```bash
    java -jar target/security-demo-0.0.1-SNAPSHOT.jar
    ```

4.  服务启动后，会监听在 `8080` 端口。

## 如何测试 API

推荐使用 **Postman** 或 **cURL** 等工具进行测试。

### 1. 访问受保护接口 (未登录)

尝试直接访问 `/api/hello`。

*   **cURL 命令:**
    ```bash
    curl -v http://localhost:8080/api/hello
    ```
*   **预期结果:**
    你会收到一个 **`401 Unauthorized`** (未授权) 状态码，证明接口已被成功保护。

### 2. 登录并获取会话

向 `/login` 接口发送 POST 请求，提交用户名和密码。

*   **cURL 命令:**
    ```bash
    # -c cookie.txt 会将会话 Cookie 保存到文件中
    curl -v -X POST -c cookie.txt \
      -d "username=test&password=123456" \
      http://localhost:8080/login
    ```
*   **预期结果:**
    你会收到一个 **`302 Found`** (重定向) 响应，表示登录成功。一个名为 `JSESSIONID` 的 Cookie 会被保存在 `cookie.txt` 文件中。

### 3. 访问受保护接口 (已登录)

携带刚才获取到的 Cookie，再次访问 `/api/hello`。

*   **cURL 命令:**
    ```bash
    # -b cookie.txt 会将 Cookie 发送给服务器
    curl -v -b cookie.txt http://localhost:8080/api/hello
    ```
*   **预期结果:**
    你会收到一个 **`200 OK`** 响应，并且响应体的内容是：
    ```
    Hello World
    ```

完成这些步骤，你就完成了一次完整的认证和授权流程的测试。