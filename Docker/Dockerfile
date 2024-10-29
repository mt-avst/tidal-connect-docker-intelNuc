FROM debian:bullseye

RUN apt-get update -y -q && apt-get upgrade -y -q

RUN apt install --fix-missing --fix-broken -y -q \
    git \
    libavformat58 \
    libportaudio2 \
    libflac++6v5 \
    libavahi-common3 \
    libavahi-client3 \
    alsa-utils \
    curl \
    portaudio19-dev \
    neovim \
    zsh \
    tmux \
    pulseaudio

RUN mkdir -p /app/ifi-tidal-release
WORKDIR /app/ifi-tidal-release

COPY src/bin/ bin
COPY src/pa_devs/ .
COPY src/play .
COPY src/id_certificate id_certificate
COPY src/licenses licenses
RUN chmod a+x bin/* 

# Configure ALSA to use the optical audio output by default
RUN cat <<EOF > /etc/asound.conf
pcm.!default {
  type hw
  card 0
  device 1
}

ctl.!default {
  type hw
  card 0
}
EOF

ENV PA_ALSA_PLUGHW=1

COPY entrypoint.sh /
RUN chmod a+x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
