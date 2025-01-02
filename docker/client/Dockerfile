FROM nvidia/cuda:12.6.3-cudnn-devel-ubuntu22.04
# FROM nvidia/cuda:11.8.0-cudnn8-runtime-ubuntu22.04
# FROM nvidia/cuda:11.8.0-runtime-ubuntu22.04
# 

ENV DEBIAN_FRONTEND=noninteractive 

WORKDIR /app

# Установите необходимые пакеты и добавьте репозиторий deadsnakes
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y software-properties-common git && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y python3.10 python3.10-venv python3.10-dev python3-tk libgl1 ffmpeg \
                       libgtk2.0-dev pkg-config libglib2.0-0 libsm6 libxrender1 libxext6 \
                       libgtk-3-dev

# Скопируйте файлы проекта
COPY ./requirements.txt ./
# COPY ./Roop/requirements.txt ./Roop/requirements.txt
COPY ./Roop/requirements-headless.txt ./Roop/requirements-headless.txt

# Создайте виртуальное окружение и установите зависимости
# RUN python3.10 -m venv venv && \
#     sed -i 's/include-system-site-packages = false/include-system-site-packages = true/' venv/pyvenv.cfg && \
#     . venv/bin/activate && \
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && \
    python3.10 get-pip.py && \
    rm get-pip.py && \
    pip install --upgrade pip

RUN pip install --no-cache-dir -r requirements.txt && \
    pip install --no-cache-dir -r Roop/requirements-headless.txt
    # pip install -r Roop/requirements.txt && \
RUN pip install --no-cache-dir \
    torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118 && \
    pip install --no-cache-dir onnxruntime-gpu tyro pykalman
    # onnxruntime tensorflow customtkinter tkinterdnd2 opencv-python insightface opennsfw2
# Команда для запуска контейнера
# CMD ["bash", "-c", "sleep infinity"]
CMD ["bash", "-c", "while true; do python3.10 client.py --update;python3.10 client.py; sleep 30; done"]