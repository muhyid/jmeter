FROM registry.access.redhat.com/ubi8/openjdk-11:1.13
ENV JMETER_VERSION "5.5"

LABEL jmeter.version=${JMETER_VERSION}

# Set JMeter base and home
ENV JMETER_BASE /opt/jmeter
ENV JMETER_HOME $JMETER_BASE/apache-jmeter-${JMETER_VERSION}
# Add JMeter to the Path
ENV PATH $JMETER_HOME/bin:$PATH

# Create jmeter directory with tests and results folder and install JMeter
RUN mkdir -p $JMETER_BASE/{tests,results}

# Download latest jmeter binaries from the Apache archive:
RUN curl -SL https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-${JMETER_VERSION}.zip -o $JMETER_BASE/apache-jmeter-${JMETER_VERSION}.zip
RUN cd $JMETER_BASE && unzip apache-jmeter-${JMETER_VERSION}.zip && rm -rf apache-jmeter-${JMETER_VERSION}.zip

############# ARE PLUGINS NEEDED?
# COPY binaries/JMeterPlugins-ExtrasLibs-1.4.0.zip $JMETER_BASE/apache-jmeter-${JMETER_VERSION}/

# Set working directory
WORKDIR $JMETER_BASE

# Entrypoint has same signature as "jmeter" command
COPY run.sh $JMETER_BASE/run.sh

USER 0

RUN chgrp -R 0 $JMETER_BASE \
    && chmod -R g+rw $JMETER_BASE \
    && chmod gu+x $JMETER_BASE/run.sh

#1001 is an arbitrary choice
USER 1001

ENTRYPOINT ["sh", "-c", "source ${JMETER_BASE}/run.sh && /bin/bash"]

# CMD ["/bin/bash"]
