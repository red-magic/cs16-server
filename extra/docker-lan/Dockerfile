FROM debian:13

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y lib32stdc++6 curl xz-utils && \
    mkdir -v /tmp/cs16-server && \
    cd /tmp/cs16-server && \
    curl -LO https://steamcdn-a.akamaihd.net/client/steamcmd_linux.tar.gz && \
    curl -LO https://github.com/yapb/yapb/releases/download/4.4.957/yapb-4.4.957-linux.tar.xz && \
    useradd -r -m -s "$(command -v nologin)" -d /var/hlds -c "Counter-Strike 1.6 Dedicated Server" hlds

USER hlds

RUN mkdir -v /var/hlds/.steam && \
    cd /var/hlds/.steam && \
    ln -sfv . steam && \
    tar xvf /tmp/cs16-server/steamcmd_linux.tar.gz && \
    ./steamcmd.sh +force_install_dir cs16 +login anonymous +app_update 90 validate +quit && \
    ./steamcmd.sh +force_install_dir cs16 +login anonymous +app_update 90 validate +quit && \
    ./steamcmd.sh +force_install_dir cs16 +login anonymous +app_update 90 validate +quit && \
    tar xvf /tmp/cs16-server/yapb-4.4.957-linux.tar.xz && \
    chmod -v 755 addons/yapb/data/* && \
    mv -vf addons cs16/cstrike && \
    ln -sfv linux32 sdk32 && \
    touch cs16/cstrike/{listip.cfg,banned.cfg}

USER hlds

RUN cd /var/hlds/.steam && \
    sed -i 's|yb_radio_mode "2"|yb_radio_mode "0"|' cs16/cstrike/addons/yapb/conf/yapb.cfg && \
    sed -i 's|yb_difficulty "3"|yb_difficulty "0"|' cs16/cstrike/addons/yapb/conf/yapb.cfg && \
    sed -i 's|yb_quota "0"|yb_quota "16"|' cs16/cstrike/addons/yapb/conf/yapb.cfg && \
    sed -i 's|yb_quota_mode "normal"|yb_quota_mode "fill"|' cs16/cstrike/addons/yapb/conf/yapb.cfg

USER hlds

RUN cd /var/hlds/.steam && \
    sed -i 's|mp_timelimit 20|mp_timelimit 0|' cs16/cstrike/server.cfg && \
    echo "rcon_password qwerty123456" >> cs16/cstrike/server.cfg

USER root

RUN rm -rfv /tmp/cs16-server && \
    rm -vrf /var/lib/apt/lists/*

USER hlds

WORKDIR /var/hlds/.steam/cs16

EXPOSE 27015/tcp 27015/udp

CMD ["./hlds_run", "-game", "cstrike", "-secure", "-pingboost", "3", "+sv_lan", "1", "+map", "de_dust2", "-dll", "cstrike/addons/yapb/bin/yapb.so"]
