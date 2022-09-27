FROM biometrioearth/dev:jupylab_base_0.1
USER root
ENV DEB_PACKAGES="python3-gdal graphviz graphviz-dev"
#ipywidgets is required by pgmpy
ENV PIP_PACKAGES_COMMON="numpy matplotlib geopandas rasterio fiona geojson pysal osmnx pyrosm geopy folium pandas-bokeh pygraphviz ipywidgets pgmpy causalnex"

RUN apt-get update && apt-get install -y $DEB_PACKAGES

RUN sed -i "s/c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py

#effectively load password and configs
RUN cp ~/.jupyter/jupyter_notebook_config.py ~/.jupyter/jupyter_server_config.py

RUN pip install $PIP_PACKAGES_COMMON

VOLUME ["/shared_volume"]

WORKDIR /

ENV NB_PREFIX eiurl

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=eiurl"]
