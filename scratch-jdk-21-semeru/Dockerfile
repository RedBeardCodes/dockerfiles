
FROM ibm-semeru-runtimes:open-21-jdk-noble AS builder

RUN jlink \
    --module-path "$JAVA_HOME/jmods" \
    --add-modules ALL-MODULE-PATH \
    --output /opt/java/runtime

RUN mkdir -p /opt/scratch-root/tmp \
    && chmod 1777 /opt/scratch-root/tmp

FROM scratch

COPY --from=builder /opt/java/runtime /opt/java/runtime

COPY --from=builder /lib64/ld-linux-x86-64.so.2 \
    /lib64/ld-linux-x86-64.so.2

COPY --from=builder /lib/x86_64-linux-gnu /lib/x86_64-linux-gnu

COPY --from=builder /opt/scratch-root/tmp /tmp

USER 10001:10001

ENV JAVA_HOME=/opt/java/runtime
ENV PATH=/opt/java/runtime/bin
ENV JAVA_TOOL_OPTIONS="-Djava.io.tmpdir=/tmp"

ENTRYPOINT ["java"]

