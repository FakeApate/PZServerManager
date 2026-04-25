###########################################################
# Dockerfile that builds a Project Zomboid Gameserver
###########################################################
FROM cm2network/steamcmd:root

RUN mkdir -p /opt/pzserver \
    && chown steam:steam /opt/pzserver

COPY --chown=steam:steam update_zomboid.txt /home/steam/update_zomboid.txt

USER steam
WORKDIR /opt/pzserver

RUN bash "${STEAMCMDDIR}/steamcmd.sh" +runscript /home/steam/update_zomboid.txt

EXPOSE 16261/udp
EXPOSE 16262/udp
EXPOSE 27015/tcp

ENTRYPOINT ["bash", "/opt/pzserver/start-server.sh"]
