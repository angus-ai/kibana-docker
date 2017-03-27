FROM docker.elastic.co/kibana/kibana-ubuntu-base:latest
MAINTAINER Elastic Docker Team <docker@elastic.co>
ARG KIBANA_DOWNLOAD_URL
EXPOSE 5601

WORKDIR /usr/share/kibana
RUN curl -Ls ${KIBANA_DOWNLOAD_URL} | tar --strip-components=1 -zxf - && \
    ln -s /usr/share/kibana /opt/kibana

# Set some Kibana configuration defaults.
ADD config/kibana.yml /usr/share/kibana/config/

# Add the launcher/wrapper script. It knows how to interpret environment
# variables and translate them to Kibana CLI options.
ADD bin/kibana-docker /usr/local/bin/

ADD custom_files/grid.js /usr/share/kibana/src/core_plugins/kibana/public/dashboard/directives/

# Add a self-signed SSL certificate for use in examples.
ADD ssl/kibana.example.org.* /usr/share/kibana/config/

RUN usermod --home /usr/share/kibana kibana

RUN /usr/share/kibana/bin/kibana-plugin install https://github.com/angus-ai/kibana_ext_metrics_vis/releases/download/0.1.1/kibana_ext_metrics_vis-0.1.1.zip

RUN /usr/share/kibana/bin/kibana-plugin install https://github.com/angus-ai/kbn_c3js_vis/releases/download/v0.0.1/kbn_c3js_vis-0.0.1.zip

RUN /usr/share/kibana/bin/kibana-plugin install https://github.com/angus-ai/kibana-html-plugin/releases/download/v0.0.4/kibana-html-plugin-0.0.4.zip

ENV PATH=/usr/share/kibana/bin:$PATH
CMD /usr/local/bin/kibana-docker
