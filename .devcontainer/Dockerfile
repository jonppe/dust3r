# Container for training Mambara with additional dev tools (e.g. jupyter)
FROM nvcr.io/nvidia/cuda:12.0.1-devel-ubuntu22.04

RUN chmod 1777 /tmp/ && apt-get -y update && \
    apt-get -y install wget git cmake python3-dev python3-pip python3-wheel python3-setuptools-whl python3-packaging python3-venv \
    libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6 libglib2.0-0 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

 
WORKDIR /workspace
RUN python3 -m venv .venv && ln -s /usr/lib/x86_64-linux-gnu/libcuda.so.1 /usr/lib/x86_64-linux-gnu/libcuda.so

ENV PATH="/workspace/.venv/bin:$PATH"

# install packaging stuff first since torch seems to prefer that.
RUN pip install packaging setuptools wheel && pip install torch torchvision
COPY requirements.txt ./
RUN pip install -r requirements.txt
COPY . ./

#  curope is optional. Also, this probably is not enough for Python to find the modules
#ENV TORCH_CUDA_ARCH_LIST="8.6"
#RUN cd croco/models/curope/ && python setup.py build_ext --inplace

CMD /bin/bash
