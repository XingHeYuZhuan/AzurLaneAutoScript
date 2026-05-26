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

RUN find /install -type d -name "tests" -exec rm -rf {} + \
    && find /install -type d -name "test" -exec rm -rf {} + \
    && find /install -type d -name "__pycache__" -exec rm -rf {} + \
    && find /install -type d -name "*.egg-info" -exec rm -rf {} + \
    && find /install -name "*.pyc" -delete \
    && find /install -name "*.pyo" -delete \
    && find /install -name "*.exe" -delete \
    && find /install -name "*.whl" -delete \
    && find /install -name "*.a" -delete \
    && rm -rf /install/lib/python3.7/site-packages/matplotlib/mpl-data/fonts

FROM python:3.7-slim-bullseye

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

COPY --from=builder /install /usr/local

RUN find /usr/local -type d -name "__pycache__" -exec rm -rf {} + \
    && rm -rf /usr/share/doc/* /usr/share/man/* /usr/share/locale/* \
    && rm -rf /var/cache/debian-installer/*

CMD ["python", "gui.py"]