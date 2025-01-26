在 Ubuntu 上运行 Spring Boot 应用的步骤如下：

### 1. **安装 Java（JDK）**

Spring Boot 需要 Java 运行环境，通常是 JDK 8 或更高版本。首先检查是否已安装 Java，或者安装最新的 JDK。

#### 1.1 检查 Java 是否已安装

打开终端并运行以下命令检查 Java 版本：

```bash
java -version
```

如果没有安装 Java，可以通过以下命令安装。

#### 1.2 安装 OpenJDK（以 OpenJDK 11 为例）

```bash
sudo apt update
sudo apt install openjdk-11-jdk
```

安装完成后，设置 `JAVA_HOME` 环境变量。

#### 1.3 配置 `JAVA_HOME`

编辑 `.bashrc` 文件（或 `.zshrc` 如果你使用的是 Zsh）来设置 `JAVA_HOME` 环境变量。

```bash
nano ~/.bashrc
```

添加以下行：

```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

保存并退出，然后运行以下命令使更改生效：

```bash
source ~/.bashrc
```

#### 1.4 验证 Java 安装

验证 JDK 安装：

```bash
java -version
```

### 2. **安装 Maven（可选）**

如果你的 Spring Boot 项目使用 Maven 构建，可以安装 Maven 来构建和运行应用。

#### 2.1 安装 Maven

```bash
sudo apt install maven
```

#### 2.2 验证 Maven 安装

```bash
mvn -v
```

### 3. **构建 Spring Boot 项目**

Spring Boot 项目通常有两种构建方式：

- 使用 Maven 构建
- 使用 Gradle 构建

#### 3.1 使用 Maven 构建

进入你的 Spring Boot 项目目录，并运行以下命令来构建项目：

```bash
mvn clean install
```

构建完成后，会生成一个 `.jar` 文件（通常位于 `target` 目录下）。

#### 3.2 使用 Gradle 构建

如果使用 Gradle 构建项目，进入项目目录并运行：

```bash
./gradlew build
```

构建完成后，会生成一个 `.jar` 文件（通常位于 `build/libs` 目录下）。

### 4. **运行 Spring Boot 应用**

无论你使用 Maven 还是 Gradle，最终你都会得到一个可执行的 `.jar` 文件。你可以通过以下命令来运行 Spring Boot 应用：

```bash
java -jar target/your-application-name.jar
```

确保 `your-application-name.jar` 替换为你实际的 `.jar` 文件名。

### 5. **后台运行 Spring Boot 应用（可选）**

如果你希望将 Spring Boot 应用在后台运行，可以使用 `nohup` 或 `screen` 来确保应用在后台运行。

#### 5.1 使用 `nohup`：

```bash
nohup java -jar target/your-application-name.jar &
```

这将把应用在后台运行，并将输出重定向到 `nohup.out` 文件中。

#### 5.2 使用 `screen`：

```bash
screen -S springboot-app
java -jar target/your-application-name.jar
```

按 `Ctrl + A` 然后按 `D` 退出屏幕会话，应用仍然在后台运行。

### 6. **访问 Spring Boot 应用**

默认情况下，Spring Boot 应用会在 `8080` 端口上运行。如果你没有更改端口配置，可以通过以下方式访问应用：

```bash
http://localhost:8080
```

如果你需要更改端口，可以在 `application.properties` 或 `application.yml` 文件中修改：

```properties
server.port=8081
```

### 7. **停止 Spring Boot 应用**

如果你是通过 `nohup` 或 `screen` 运行的 Spring Boot 应用，你可以通过以下命令停止它：

#### 7.1 使用 `kill` 命令：

首先找到应用的进程 ID（PID）：

```bash
ps aux | grep your-application-name.jar
```

然后使用 `kill` 命令终止进程：

```bash
kill -9 <PID>
```

#### 7.2 使用 `screen`：

如果使用 `screen` 运行应用，可以重新连接到后台会话并停止应用：

```bash
screen -r springboot-app
```

然后按 `Ctrl + C` 停止应用。

### 8. **自动化启动（可选）**

如果希望 Spring Boot 应用在系统启动时自动启动，可以使用 `systemd` 服务进行配置，或者使用其他类似的工具来管理应用。

#### 8.1 创建 systemd 服务

1. 创建一个新的 `systemd` 服务文件：

```bash
sudo nano /etc/systemd/system/springboot-app.service
```

2. 在文件中添加以下内容：

```ini
[Unit]
Description=Spring Boot Application
After=network.target

[Service]
User=ubuntu
ExecStart=/usr/bin/java -jar /path/to/your-application-name.jar
SuccessExitStatus=143
TimeoutStopSec=10
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

3. 保存并关闭文件。
4. 重新加载 `systemd` 服务：

```bash
sudo systemctl daemon-reload
```

5. 启动并启用服务：

```bash
sudo systemctl start springboot-app
sudo systemctl enable springboot-app
```

6. 检查服务状态：

```bash
sudo systemctl status springboot-app
```

7. 如果需要停止服务，可以使用：

```bash
sudo systemctl stop springboot-app
```

### 总结

在 Ubuntu 上运行 Spring Boot 应用的过程相对简单，只需要安装 Java、构建项目并使用 `java -jar` 命令启动应用。如果希望应用在后台运行，可以使用 `nohup` 或 `screen` 工具，或者通过 `systemd` 设置自动启动服务。
