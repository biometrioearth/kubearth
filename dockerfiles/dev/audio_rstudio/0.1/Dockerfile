# Use RStudio as the base image
FROM rocker/rstudio:latest

# Switch to root to install additional dependencies
USER root

# Set environment variables for installation and configuration
ENV DEB_BUILD_DEPS="software-properties-common gfortran libopenblas-dev curl gnupg apt-transport-https ca-certificates"
ENV R_DEB_BUILD_DEPS="r-base libgit2-dev libgmp3-dev libmpfr-dev libssl-dev libxml2-dev libcurl4-openssl-dev libfontconfig1-dev libharfbuzz-dev  libfribidi-dev libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev"
ENV R_SITE_LIBRARY="/usr/local/lib/R/site-library"
ENV R_PACKAGES="seewave soundecology ggplot2 viridis repr IRdisplay evaluate crayon pbdZMQ devtools uuid digest IRkernel"

# Update system and install dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends $DEB_BUILD_DEPS

# Download and add the GPG key for R repository securely
RUN curl -fsSL https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | gpg --dearmor -o /usr/share/keyrings/cloud-r-project-org.gpg

# Add the R repository using the signed-by option with the new key
RUN echo "deb [signed-by=/usr/share/keyrings/cloud-r-project-org.gpg] https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/" > /etc/apt/sources.list.d/cloud-r-project-org.list

# Proceed with system update and install R dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends $R_DEB_BUILD_DEPS

# Install R packages
RUN R -e "install.packages(c('seewave', 'monitoR', 'soundecology', 'ggplot2', 'viridis', 'repr', 'IRdisplay', 'evaluate', 'crayon', 'pbdZMQ', 'devtools', 'uuid', 'digest', 'IRkernel'), repos='http://cran.rstudio.com/')"

# Set up volume for shared data, useful for persistent or shared data
VOLUME ["/shared_volume"]

# Set work directory, you can adjust this as needed
WORKDIR /