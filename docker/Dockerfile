# 使用Ubuntu 20.04作为基础镜像
FROM ubuntu:20.04

# 避免安装过程中出现交互式提示
ARG DEBIAN_FRONTEND=noninteractive

# 安装依赖，更新包列表
RUN apt-get update && apt-get install -y \
    wget \
    bzip2 \
    ca-certificates \
    git \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    && rm -rf /var/lib/apt/lists/*

# 设置工作目录为/app
WORKDIR /app

# 下载Miniconda安装脚本，安装Miniconda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh

# 将Conda添加到PATH，这样我们就可以直接使用conda命令
ENV PATH /opt/conda/bin:$PATH

# 复制当前目录下的文件到容器的/app目录下
COPY . /app

# 使用conda创建一个新环境并安装requirements.txt中列出的所有依赖
# 假设requirements.txt中列出的是conda能安装的包
# 如果有一些包conda找不到，可能需要使用pip安装
RUN conda create --name myenv python=3.10
RUN echo "source activate myenv" > ~/.bashrc
ENV PATH /opt/conda/envs/myenv/bin:$PATH
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

# 使容器监听端口5000
EXPOSE 5000

# 定义容器启动时执行的命令
CMD ["python", "./app.py"]
