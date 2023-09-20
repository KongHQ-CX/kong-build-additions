ARG KONG_VERSION="3.3.1.0"

FROM kong/kong-gateway:${KONG_VERSION}-ubuntu as builder
ARG PLUGIN_VERSION_TAG=3.3.1

USER root
RUN apt update && \
    apt install -y curl && \
    mkdir -p /builder

WORKDIR /builder
RUN curl -L -o handler.lua https://raw.githubusercontent.com/Kong/kong/${PLUGIN_VERSION_TAG}/kong/plugins/rate-limiting/handler.lua && \
    curl -L -o schema.lua https://raw.githubusercontent.com/Kong/kong/${PLUGIN_VERSION_TAG}/kong/plugins/rate-limiting/schema.lua && \
    sed -i 's/name = "rate-limiting",/name = "rate-limiting-2",/g' schema.lua && \
    sed -i 's/kong_meta.version/kong_meta.core_version/g' handler.lua

#---#

FROM kong/kong-gateway:${KONG_VERSION}-ubuntu as runner

# Copy patched secondary plugin from old phase
USER root
COPY --from=builder /builder/handler.lua /usr/local/share/lua/5.1/kong/plugins/rate-limiting-2/handler.lua
COPY --from=builder /builder/schema.lua /usr/local/share/lua/5.1/kong/plugins/rate-limiting-2/schema.lua
USER kong


## !! Your normal Kong build would probably proceed BELOW THIS LINE !! ##
