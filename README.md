# linuxqq
docker build -t mylinux:1 .
xhost +local:docker #要运行具有 GUI 支持的 Docker 容器，您需要配置 X11 服务器访问：
docker run -it --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix
mylinux:1
FROM debian:bookworm
# 安装依赖
RUN apt update && \
apt install -y \
x11-xserver-utils \
xdg-user-dirs \
xdg-utils \
xkb-data \
zutty \
xfonts-intl-chinese \
xfonts-wqy \
fonts-wqy-zenhei \
libasound2
WORKDIR /root
COPY QQ*.deb /root/
RUN apt install -y /root/QQ*.deb
RUN export LC_ALL=zh_CN.UTF-8
# 设置环境变量以使用主机的 X11 显示器
ENV DISPLAY=:0
RUN useradd -ms /bin/bash myuser
USER myuser
WORKDIR /home/myuser
CMD ["qq", "--no-sandbox"]
