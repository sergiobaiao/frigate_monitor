ARG BUILD_FROM
FROM ${BUILD_FROM}

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install basics for running arbitrary apps; add more as needed
RUN apk add --no-cache bash jq coreutils ca-certificates tzdata

# --- Place your app install here ---
# Example 1: copy local files (uncomment and adjust)
# COPY --chmod=755 myapp /usr/local/bin/myapp
# Example 2: fetch a release tarball (uncomment and adjust)
# RUN curl -L -o /tmp/app.tgz https://example.com/myapp-v1.2.3.tgz \
#   && tar -xzf /tmp/app.tgz -C /usr/local/bin --strip-components=1 \
#   && rm /tmp/app.tgz

# Copy s6 services and init scripts
COPY rootfs /

# s6-overlay is already included in HA base images; the supervisor will start /init
