FROM biometrioearth/dev:megadetector_v5_0.1

# conabio_ml_vision installation
RUN git clone -b dev https://bitbucket.org/conabio_cmd/conabio_ml_vision.git /lib/conabio_ml_vision
RUN pip install -r /lib/conabio_ml_vision/requirements.txt
RUN git clone -b conabio_ml_core https://bitbucket.org/conabio_cmd/conabio_ml.git /lib/conabio_ml_vision/conabio_ml
RUN pip install -r /lib/conabio_ml_vision/conabio_ml/requirements.txt

ENV PYTHONPATH=$PYTHONPATH:/lib/conabio_ml_vision:/lib/conabio_ml_vision/conabio_ml

#overwrite password
RUN sed -i "s/c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py

VOLUME ["/shared_volume"]

WORKDIR /

ENV NB_PREFIX img_video

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=img_video"]
