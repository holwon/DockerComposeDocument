# 使用最新的 Ubuntu 作为基础镜像
FROM ubuntu:latest

# 添加其他安装步骤
RUN apt-get update 

RUN apt-get install ca-certificates -y

RUN apt-get update

# 清理APT缓存
RUN rm -rf /var/lib/apt/lists/*

# 更新软件源
RUN apt-get update

# 安装 wget、libx11-xcb1 和 tightvncserver
RUN apt-get install -y wget libx11-xcb1 tightvncserver apt-transport-https software-properties-common gnupg

# 下载 VS Code 1.85.2 版本的安装包
RUN wget https://update.code.visualstudio.com/1.85.2/linux-deb-x64/stable -O vscode.deb

# 安装 VS Code
RUN dpkg -i vscode.deb || true
RUN apt-get install -f -y

# 清理安装包
RUN rm vscode.deb

# 创建一个非 root 用户
RUN useradd -m vscode

# 设置 VNC 密码
RUN mkdir ~/.vnc
RUN echo "mypassword" | vncpasswd -f > ~/.vnc/passwd
RUN chmod 600 ~/.vnc/passwd

# 设置 USER 环境变量
ENV USER root

ENV DONT_PROMPT_WSL_INSTALL 1


# 切换到非 root 用户
USER vscode

# 在 Dockerfile 中添加以下命令来创建 .Xauthority 文件
RUN touch /home/vscode/.Xauthority
RUN chown vscode:vscode /home/vscode/.Xauthority

# 启动 VNC 服务器并运行 VS Code
CMD vncserver :1 -geometry 1280x800 -depth 24 && DISPLAY=:1 code --no-sandbox --user-data-dir=/home/vscode/.vscode

