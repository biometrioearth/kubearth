#FROM biometrioearth/dev:jupylab_base_0.1
#next img is sipecam/audio-dgpi-kale-tensorflow-yuntu-dask-gpu-cert:0.6.1_alfresco
FROM biometrioearth/dev:yuntu_0.6.1_alfresco
USER root
#ipywidgets is required by pgmpy
ENV PIP_PACKAGES_COMMON="scikit-maad"

#--generate-config is not needed if at some point in the future FROM biometrioearth/dev:jupylab_base_0.1 is used
RUN jupyter notebook --generate-config && \
    sed -i "s/c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py

#effectively load password and configs
RUN cp ~/.jupyter/jupyter_notebook_config.py ~/.jupyter/jupyter_server_config.py

RUN pip install $PIP_PACKAGES_COMMON

VOLUME ["/shared_volume"]

WORKDIR /

ENV NB_PREFIX audio

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=audio"]
