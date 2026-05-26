# ==========================================
# 阶段 1：构建环境 (Builder)
# ==========================================
FROM python:3.7-slim-bullseye AS builder

WORKDIR /tmp/build

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    pkg-config \
    git \
    libavformat-dev \
    libavcodec-dev \
    libavdevice-dev \
    libavutil-dev \
    libswscale-dev \
    libswresample-dev \
    libavfilter-dev \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

RUN pip install --no-cache-dir --upgrade pip wheel setuptools \
    && pip install --no-cache-dir Cython==0.29.37 \
    && pip install --no-cache-dir av==10.0.0 --no-build-isolation \
    && pip install --no-cache-dir --prefix=/install -r requirements.txt

# ==========================================
# 阶段 2：运行环境 (Runner) - 最终镜像
# ==========================================
FROM python:3.7-slim-bullseye

# 设置工作目录
WORKDIR /app/AzurLaneAutoScript

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    adb \
    openssh-client \
    libgomp1 \
    libavformat58 \
    libavcodec58 \
    libavdevice58 \
    libavutil56 \
    libswscale5 \
    libswresample3 \
    libavfilter7 \
    && rm -rf /var/lib/apt/lists/* \
    && git config --global --add safe.directory '*'

# 只把编译好的 Python 依赖库复制过来
COPY --from=builder /install /usr/local


CMD ["python", "gui.py"]