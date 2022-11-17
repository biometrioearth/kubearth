FROM biometrioearth/dev:mdv5_jupylab_base_0.1

ENV DEB_PACKAGES="ffmpeg libsm6 libxext6"
ENV PIP_PACKAGES_COMMON="jsonpickle humanfriendly PyYAML>=5.3.1"
#ENV PIP_PACKAGES_COMMON="jsonpickle humanfriendly PyYAML>=5.3.1 torch==1.10.1 torchvision==0.11.2"
#next according to: https://pytorch.org/get-started/previous-versions/
ENV PIP_PACKAGES_TORCH="torch==1.10.1+cu113 torchvision==0.11.2+cu113 torchaudio==0.10.1 -f https://download.pytorch.org/whl/cu113/torch_stable.html"
ENV PYTHONPATH=$PYTHONPATH:/lib/megadetector/cameratraps:/lib/megadetector/ai4eutils:/lib/megadetector/yolov5
ENV MEGADETECTOR_EXEC=/lib/megadetector/cameratraps/detection/run_detector_batch.py
ENV PYTHON_EXEC=python3

RUN apt-get update && apt-get install -y $DEB_PACKAGES && rm -rf /var/lib/apt/lists/*

# Megadetector installation
WORKDIR /lib/megadetector
RUN git clone https://github.com/Microsoft/cameratraps /lib/megadetector/cameratraps
RUN git clone https://github.com/Microsoft/ai4eutils /lib/megadetector/ai4eutils
# For MDv5
RUN git clone https://github.com/ultralytics/yolov5/ /lib/megadetector/yolov5
RUN cd /lib/megadetector/yolov5 && git checkout c23a441c9df7ca9b1f275e8c8719c949269160d1

#pip packages
RUN pip install $PIP_PACKAGES_COMMON
RUN pip install $PIP_PACKAGES_TORCH

VOLUME ["/shared_volume"]

WORKDIR /

ENV NB_PREFIX img_video

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=img_video"]
