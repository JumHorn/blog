# debian docker image

    使用了9年半的笔记本坏了，笔记本一直用的debian系统，现在又不想买笔记本，又离不开debian系统
    所以在此制作docker镜像并推送到dockerhub,来纪念我使用过的debian(bullseye)

# Dockerfile

```Dockerfile
# 基础镜像
FROM debian:bullseye-slim

# 安装必要的软件包
RUN apt-get update && apt-get install -y \
    x11vnc \
    websockify \
    novnc

# 配置novnc服务的端口号
EXPOSE 8080

# 运行novnc服务
CMD ["websockify", "--web=/usr/share/novnc/", "8080", "--", "x11vnc", "-create", "-forever"]
```

## Reference

1. https://github.com/prbinu/novnc-desktop