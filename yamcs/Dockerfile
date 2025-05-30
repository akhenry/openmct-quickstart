FROM maven:3.9.9-eclipse-temurin-17 

WORKDIR /yamcs

# Clone the quickstart repository
RUN git clone https://github.com/yamcs/quickstart.git

# Update the pom.xml file to include the Yamcs Prometheus plugin
RUN sed -i '/<\/dependencies>/i \
    <!-- Yamcs Prometheus plugin -->\
    <dependency>\
      <groupId>org.yamcs</groupId>\
      <artifactId>yamcs-prometheus</artifactId>\
      <version>1.3.0</version>\
    </dependency>' /yamcs/quickstart/pom.xml

# Compile the project
RUN cd quickstart && mvn compile

# Update the yamcs.yaml configuration
RUN sed -i '/args:/a \      contextPath: "/yamcs"' /yamcs/quickstart/src/main/yamcs/etc/yamcs.yaml

# Update the yamcs.yaml configuration to include additional parameters for the HttpServer service
# Update the yamcs.yaml configuration file
RUN sed -i '/allowCredentials: false/a \ \ \ \ \ \ maxContentLength: 20000000\n\ \ \ \ \ \ webSocket:\n\ \ \ \ \ \ \ \ writeBufferWaterMark: { low: 32768, high: 160000000 }' /yamcs/quickstart/src/main/yamcs/etc/yamcs.yaml

# Command to run Yamcs
CMD ["sh", "-c", "cd /yamcs/quickstart && mvn yamcs:run"]
