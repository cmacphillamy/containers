FROM mambaorg/micromamba:1.5.1

USER root

COPY environment.yaml /tmp/environment.yaml

RUN micromamba install -y -n base -f /tmp/environment.yaml
RUN micromamba clean --all --yes
RUN apt-get update && apt-get -y install tabix

ENV PATH="$PATH:/opt/conda/bin"