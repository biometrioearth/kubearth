FROM biometrioearth/dev:jupylab_base_0.1
USER root
ENV PIP_PACKAGES_R="rpy2"
#next to avoid error when loading rpy2
ENV LD_LIBRARY_PATH=/usr/local/lib/R/lib/
#for add-apt-repository
ENV DEB_BUILD_DEPS="software-properties-common"
#for R
ENV R_KEY="E298A3A825C0D65DFD57CBB651716619E084DAB9"
ENV R_DEB_BUILD_DEPS="r-base libgit2-dev dirmngr libgmp3-dev libmpfr-dev libssl-dev libxml2-dev libcurl4-openssl-dev libfontconfig1-dev libharfbuzz-dev  libfribidi-dev libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev"
ENV R_SITE_LIBRARY="/usr/local/lib/R/site-library"
ENV R_PACKAGES="var=\"seewave soundecology repr IRdisplay evaluate crayon pbdZMQ devtools uuid digest\""

RUN apt-get install -y $DEB_BUILD_DEPS

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys $R_KEY && \
    add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/" && \
    apt-get update && \
    apt install -y --no-install-recommends $R_DEB_BUILD_DEPS

#overwrite password
RUN sed -i "s/c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:115e429a919f:21911277af52f3e7a8b59380804140d9ef3e2380'/" ~/.jupyter/jupyter_notebook_config.py

#effectively load password and configs
RUN cp ~/.jupyter/jupyter_notebook_config.py ~/.jupyter/jupyter_server_config.py

RUN pip install $PIP_PACKAGES_R

#r kernel for jupyterlab and packages
RUN echo $R_PACKAGES >  ~/.Renviron && \
    sudo chmod gou+w $R_SITE_LIBRARY && \
    R -e 'install.packages(strsplit(system("echo $var", intern=TRUE)," ")[[1]], lib="/usr/local/lib/R/site-library/")' && \
    R -e 'devtools::install_github("IRkernel/IRkernel")' && \
    sudo R -e 'IRkernel::installspec(user=FALSE)'

RUN sudo chmod gou+wx -R ~/.local

VOLUME ["/shared_volume"]

WORKDIR /

ENV NB_PREFIX audio

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=audio"]
