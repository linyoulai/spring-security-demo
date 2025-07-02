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

## 如何在本地运行 (使用 IntelliJ IDEA)

### 环境要求

*   JDK 17 或更高版本
*   IntelliJ IDEA
*   Apache Maven 3.6+ (通常 IDEA 已内置)

### 运行步骤

1.  **克隆或下载项目:**
    *   使用 Git 克隆项目到本地：
      ```bash
      git clone https://github.com/你的用户名/security-demo.git
      ```
    *   或者直接下载项目的 ZIP 压缩包并解压。

2.  **在 IntelliJ IDEA 中打开项目:**
    *   选择 `File -> Open...`，然后选择你项目中的 `pom.xml` 文件，以 Maven 项目的形式导入。
    *   等待 IDEA 自动下载所有 Maven 依赖。

3.  **运行应用程序:**
    *   在项目文件树中，找到主启动类 `src/main/java/com/example/securitydemo/HelloWorldDemoApplication.java`。
    *   右键点击该文件，或者点击 `main` 方法旁的绿色三角箭头。
    *   选择 **"Run 'HelloWorldDemoApplication'"**。

4.  观察控制台输出，当看到类似 `Tomcat started on port(s): 8080 (http)` 的日志时，表示服务已成功启动。

## 如何使用 Postman 测试 API

### 1. 访问受保护接口 (未登录)

这一步验证接口确实被保护了。

1.  打开 Postman，新建一个 `GET` 请求。
2.  在地址栏输入：`http://localhost:8080/api/hello`
3.  点击 **Send**。

*   **预期结果:** 你会收到一个 **`200 ok`** 状态码，Body 中可能会显示一个 HTML 登录页面。

### 2. 登录并获取会话 Cookie

这一步模拟用户提交表单登录。

1.  新建一个 `POST` 请求。
2.  在地址栏输入：`http://localhost:8080/login`
3.  切换到 **Body** 标签页，选择 **`x-www-form-urlencoded`**。
4.  在下方的表格中，添加两条数据：
    *   **KEY:** `username`, **VALUE:** `test`
    *   **KEY:** `password`, **VALUE:** `123456`
5.  点击 **Send**。

*   **预期结果:** Postman 会自动处理重定向，最终你会收到一个 `200 OK` 的响应。最重要的是，检查响应界面的 **Cookies** 标签页，你应该会看到一个由服务器设置的 `JSESSIONID`。Postman 会自动保存这个 Cookie 用于后续请求。

### 3. 访问受保护接口 (已登录)

现在，带着登录成功的“身份”，再次访问受保护的接口。

1.  切换回**第一个**测试 `/api/hello` 的请求标签页。
2.  **不需要做任何修改**，直接再次点击 **Send**。

*   **预期结果:**
    *   状态码会变为 **`200 OK`**。
    *   响应的 **Body** 部分会显示字符串：`Hello World`。

