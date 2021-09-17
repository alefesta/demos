FROM intel/intel-optimized-tensorflow-avx512 AS base

COPY mlflow /fuseml-dockerfiles
RUN apt-get update && apt-get -y install wget curl git gpg 

ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8
ENV PATH /opt/conda/bin:$PATH

ENV BACKEND_URI sqlite:////mlflow/mlflow.db
ENV ARTIFACT_ROOT /mlflow/artifacts
ENV WORKERS 4

EXPOSE 80

CMD mlflow server --workers ${WORKERS} --backend-store-uri ${BACKEND_URI} --default-artifact-root ${ARTIFACT_ROOT} --host 0.0.0.0 --port 80 --expose-prometheus ${PROMETHEUS_METRICS:-''}

ARG MLFLOW_VERSION=1.18.0


RUN curl https://repo.anaconda.com/pkgs/misc/gpgkeys/anaconda.asc | gpg --dearmor > conda.gpg && \
    install -o root -g root -m 644 conda.gpg /usr/share/keyrings/conda-archive-keyring.gpg && \
    gpg --keyring /usr/share/keyrings/conda-archive-keyring.gpg --no-default-keyring --fingerprint 34161F5BF5EB1D4BFBBB8F0A8AEB4F8B29D82806 && \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/conda-archive-keyring.gpg] https://repo.anaconda.com/pkgs/misc/debrepo/conda stable main" > /etc/apt/sources.list.d/conda.list && \
    apt update && apt install -y conda && apt-get clean

# ----- squeeze
FROM base

ENV FUSEML_FILES=/fuseml-dockerfiles

COPY mlflow ${FUSEML_FILES}/

COPY run.sh /usr/local/bin/run

ENTRYPOINT [ "run" ]