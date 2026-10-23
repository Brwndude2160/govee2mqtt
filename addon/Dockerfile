ARG BUILD_FROM=ghcr.io/home-assistant/base-debian:bookworm
ARG GOVEE_IMAGE=ghcr.io/brwndude2160/govee2mqtt:latest
FROM ${GOVEE_IMAGE} AS govee2mqtt
FROM ${BUILD_FROM}
COPY run.sh /run.sh
COPY --from=govee2mqtt /app/govee /app/
COPY --from=govee2mqtt /app/assets /app/assets/
COPY --from=govee2mqtt /app/AmazonRootCA1.pem /app/
CMD [ "/run.sh" ]

LABEL \
  org.opencontainers.image.title="Home Assistant Add-on: Govee to MQTT Bridge" \
  org.opencontainers.image.description="Acts as a bridge between Govee devices and Home Assistant, via the Home Assistant MQTT Integration." \
  org.opencontainers.image.source="https://github.com/Brwndude2160/govee2mqtt" \
  org.opencontainers.image.licenses="MIT"
