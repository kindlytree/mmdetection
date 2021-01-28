ARG PYTORCH="1.6.0"
ARG CUDA="10.1"
ARG CUDNN="7"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

ENV TORCH_CUDA_ARCH_LIST="6.0 6.1 7.0+PTX"
ENV TORCH_NVCC_FLAGS="-Xfatbin -compress-all"
ENV CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"


RUN mv /etc/apt/sources.list /etc/apt/sources.list.bk
ADD sources.list /etc/apt/
ADD hosts /etc/

#RUN apt-get update && apt-get install software-properties-common
RUN apt-get install apt-transport-https

RUN echo 'deb https://developer.download.nvidia.cn/compute/cuda/repos/ubuntu1804/x86_64 /' > /etc/apt/sources.list.d/cuda.list
#RUN apt-get install curl
#RUN add-apt-repository ppa:graphics-drivers/ppa
#RUN curl -s -L https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub | apt-key add -

RUN apt-get update && apt-get install -y ffmpeg libsm6 libxext6 git ninja-build libglib2.0-0 libsm6 libxrender-dev libxext6 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install MMCV
RUN pip install mmcv-full==latest+torch1.6.0+cu101 -f https://openmmlab.oss-accelerate.aliyuncs.com/mmcv/dist/index.html -i https://pypi.douban.com/simple

# Install MMDetection
RUN conda clean --all
#RUN git clone https://github.com/open-mmlab/mmdetection.git /mmdetection

RUN apt-get update && apt-get install nano

RUN conda install ipython
RUN conda install jupyter
RUN conda install matplotlib
RUN conda install scikit-learn


RUN git clone https://github.com/kindlytree/mmdetection.git /mmdetection

WORKDIR /mmdetection
ENV FORCE_CUDA="1"
RUN pip install -r requirements/build.txt
RUN pip install --no-cache-dir -e .
