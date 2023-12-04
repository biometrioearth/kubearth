FROM tensorflow/tensorflow:2.15.0-gpu

ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV DEB_BUILD_DEPS="python3-dev python3-pip python3-setuptools"
ENV DEB_PACKAGES="sudo vim less git wget curl ca-certificates ffmpeg libsm6 libxext6"
ENV PIP_PACKAGES_COMMON="jsonpickle humanfriendly PyYAML>=5.3.1"
ENV JUPYTERLAB="jupyter jupyterlab==3.4.3"
ENV PIP_PACKAGES_TORCH="torch==1.10.1+cu113 torchvision==0.11.2+cu113 torchaudio==0.10.1 -f https://download.pytorch.org/whl/cu113/torch_stable.html"
ENV PIP_PACKAGES_TF_KERAS="keras_cv"
ENV PYTHONPATH=$PYTHONPATH:/lib/megadetector/cameratraps:/lib/megadetector/ai4eutils:/lib/megadetector/yolov5
ENV MEGADETECTOR_EXEC=/lib/megadetector/cameratraps/detection/run_detector_batch.py
ENV PYTHON_EXEC=python

RUN apt-get update && export $DEBIAN_FRONTEND && echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get install -y $DEB_PACKAGES $DEB_BUILD_DEPS && pip3 install --upgrade pip && rm -rf /var/lib/apt/lists/*
RUN pip install $PIP_PACKAGES_COMMON
RUN pip install $PIP_PACKAGES_TORCH
RUN pip install $PIP_PACKAGES_TF_KERAS
RUN pip install $JUPYTERLAB

RUN curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash - && apt-get install -y nodejs

RUN jupyter notebook --generate-config && \
    sed -i "s/# c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:5c1fee0d2231:55836f6ad7c2796c139799dedf50d3e9d9058d2e'/" ~/.jupyter/jupyter_notebook_config.py

#effectively load password and configs
RUN cp ~/.jupyter/jupyter_notebook_config.py ~/.jupyter/jupyter_server_config.py

# Megadetector installation
WORKDIR /lib/megadetector
RUN git clone https://github.com/Microsoft/cameratraps /lib/megadetector/cameratraps
RUN git clone https://github.com/Microsoft/ai4eutils /lib/megadetector/ai4eutils
RUN git clone https://github.com/ultralytics/yolov5/ /lib/megadetector/yolov5
RUN cd /lib/megadetector/yolov5 && git checkout c23a441c9df7ca9b1f275e8c8719c949269160d1

#overwrite password
RUN sed -i "s/c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py

VOLUME ["/shared_volume"]

WORKDIR /

ENV NB_PREFIX img_video

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=img_video"]
