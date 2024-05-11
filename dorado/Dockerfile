FROM continuumio/miniconda3:latest

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 PATH=/opt/bin:/opt/conda/bin:$PATH

# update ubuntu packages
RUN apt-get update --fix-missing && \
    yes|apt-get upgrade && \
    apt-get install -y \
        wget \
        bzip2 \
        make \
        g++ \
        libboost-graph-dev && \
    rm -rf /bar/lib/apt/lists/*

WORKDIR /opt/bin

# Install anaconda
RUN conda config --add channels defaults && \
conda config --add channels bioconda && \
conda config --add channels conda-forge && \
conda create -n dorado python=3.10 -y

ENV PATH /opt/conda/envs/dorado/bin:$PATH
ENV CONDA_DEFAULT_ENV dorado

RUN /bin/bash -c "source activate dorado" && \
	conda install -c bioconda samtools -y && \
	conda install -c anaconda pigz -y && \
	rm -rf /opt/conda/pkgs/* && \
	echo "source activate dorado" > ~/.bashrc

RUN	cd /opt/bin && \
	wget https://cdn.oxfordnanoportal.com/software/analysis/dorado-0.5.3-linux-x64.tar.gz -P /opt/bin && \
	tar -zxvf /opt/bin/dorado-0.5.3-linux-x64.tar.gz -C /opt/bin && \
	rm /opt/bin/dorado-0.5.3-linux-x64.tar.gz

ENV PATH="/opt/bin/dorado-0.5.3-linux-x64/bin:${PATH}"
