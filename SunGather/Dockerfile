# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM

ARG BUILD_VERSION
ARG BUILD_ARCH

COPY requirements.txt ./
RUN apk add --no-cache py3-pip yq git
# pycryptodomex 3.14 currently fails to compile for arm64

# Prepare venv
RUN python3 -m venv venv

RUN source ./venv/bin/activate && \
    pip install --no-cache-dir --upgrade -r requirements.txt
#RUN pip install --no-cache-dir --upgrade pycryptodomex==3.11.0 -r requirements.txt

RUN git clone https://github.com/caliph9k/SunGather

COPY run.sh /
COPY SungrowModbusWebClient-0.3.3-py3-none-any.whl /

# Install SunGather and SungrowModbusWebClient
RUN source ./venv/bin/activate && \
    pip install --no-cache-dir /SungrowModbusWebClient-0.3.3-py3-none-any.whl

# Install requirements for add-on
RUN chmod a+x /run.sh

CMD ["/run.sh"]

LABEL \ 
    io.hass.name="SunGather-AH" \
    io.hass.description="Extract Sungrow data from local Inverter" \
    io.hass.version=${BUILD_VERSION} \
    io.hass.type="addon" \
    io.hass.arch=${BUILD_ARCH}
