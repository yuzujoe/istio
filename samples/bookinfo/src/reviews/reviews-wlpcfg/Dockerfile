# Copyright Istio Authors
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.

FROM websphere-liberty:20.0.0.6-full-java8-ibmjava

USER root

ENV SERVERDIRNAME reviews

COPY ./servers/LibertyProjectServer /opt/ibm/wlp/usr/servers/defaultServer/

RUN /opt/ibm/wlp/bin/installUtility install  --acceptLicense /opt/ibm/wlp/usr/servers/defaultServer/server.xml && \
    chmod -R g=rwx /opt/ibm/wlp/output/defaultServer/

RUN mkdir -p /usr/local/tomcat/newrelic
RUN mkdir -p /usr/local/tomcat/newrelic/logs
RUN chown -R 1000:1000 /usr/local/tomcat/newrelic/logs
COPY ./newrelic/newrelic.jar /usr/local/tomcat/newrelic/newrelic.jar
ENV JAVA_OPTS="$JAVA_OPTS -javaagent:/usr/local/tomcat/newrelic/newrelic.jar"
COPY ./newrelic/newrelic.yml /usr/local/tomcat/newrelic/newrelic.yml

ARG service_version
ARG enable_ratings
ARG star_color
ENV SERVICE_VERSION ${service_version:-v1}
ENV ENABLE_RATINGS ${enable_ratings:-false}
ENV STAR_COLOR ${star_color:-black}

RUN echo '-javaagent:/usr/local/tomcat/newrelic/newrelic.jar' >> config/jvm.options

CMD ["/opt/ibm/wlp/bin/server", "run", "defaultServer"]
