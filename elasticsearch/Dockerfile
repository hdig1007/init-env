FROM elasticsearch:2.3.5
COPY elasticsearch-analysis-ansj-2.3.5.3-release.zip /
COPY master.zip /
RUN /usr/share/elasticsearch/bin/plugin install file:///master.zip && /usr/share/elasticsearch/bin/plugin install file:///elasticsearch-analysis-ansj-2.3.5.3-release.zip
EXPOSE 9200
EXPOSE 9300
