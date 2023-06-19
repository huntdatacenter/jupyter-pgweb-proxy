FROM jupyter/minimal-notebook:lab-3.6.1

USER root
RUN set -x \
    && cd /tmp \
    && wget -q https://github.com/sosedoff/pgweb/releases/download/v0.14.0/pgweb_linux_amd64.zip -O /tmp/pgweb.zip \
    && unzip /tmp/pgweb.zip \
    && mv /tmp/pgweb_linux_amd64 /usr/local/bin/pgweb \
    && chmod a+rx /usr/local/bin/pgweb

# setup package, enable classic extension, build lab extension
# USER "${NB_USER}"
RUN mamba install -q -c conda-forge -y psycopg
RUN fix-permissions /opt/conda

WORKDIR "${HOME}/jupyter-pgweb-proxy"
COPY . .
RUN python3 -m pip install "${HOME}/jupyter-pgweb-proxy"
RUN fix-permissions /opt/conda

WORKDIR "${HOME}"
# RUN python3 -m pip install git+https://github.com/huntdatacenter/jupyter-pgweb-proxy.git
RUN jupyter serverextension enable --sys-prefix jupyter_server_proxy
RUN jupyter labextension disable "@jupyterlab/apputils-extension:announcements"

# copy configs, update permissions as root
USER root
RUN rm -rf "${HOME}/jupyter-pgweb-proxy"
RUN cp /etc/jupyter/jupyter_notebook_config.py /etc/jupyter/jupyter_notebook_config_base.py
COPY setup/jupyter_notebook_config.py /etc/jupyter/jupyter_notebook_config.py
RUN fix-permissions /etc/jupyter


USER "${NB_USER}"
