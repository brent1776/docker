FROM tdfps/ibm-base:latest
MAINTAINER Brent Jones <brent.jones@dfps.state.tx.us>

ARG APACHE_IM_HOST

USER wasadmin

RUN sudo mkdir -p /install/IM64 && \
    sudo /usr/bin/printf 'GET /IM64.tgz HTTP/1.1\r\nHost: ${APACHE_IM_HOST}\r\n\r\n' | \
      nc ${APACHE_IM_HOST} 80 | tail -n +10 | sudo tar zxf - -C /install/IM64/ && \
    /install/IM64/tools/imcl install com.ibm.cic.agent \
        -accessRights nonAdmin -acceptLicense \
        -installationDirectory /apps/ibm/installationmanager \
        -dataLocation /apps/ibm/installationmanagerData \
        -sharedResourcesDirectory /apps/ibm/installationmanager/imshared \
        -repositories /install/IM64/repository.config -showVerboseProgress \
        -preferences com.ibm.cic.common.core.preferences.preserveDownloadedArtifacts=false,com.ibm.cic.common.core.preferences.keepFetchedFiles=false \
        -log /apps/ibm/logs/IMSilentInstall.log && \
    sudo rm -rf /install/IM64

CMD ["/bin/bash"]
