# Start with the base image
FROM docker-sonic-vs

# Arguments passed during build
ARG docker_container_name
ARG need_dbg

# Copy the .deb files into the container
COPY ["debs", "/debs"]

# Remove the libswsscommon package and other dependencies to prepare for installation
RUN dpkg --remove --force-all libswsscommon
RUN apt --fix-broken install -y
RUN dpkg --purge python3-swsscommon sonic-db-cli libsaimetadata libsairedis libsaivs syncd-vs swss sonic-eventd libdashapi

# Update the package list
RUN apt-get update

# Install the required .deb packages
RUN apt install -y /debs/libdashapi_1.0.0_amd64.deb \
    /debs/libswsscommon_1.0.0_amd64.deb \
    /debs/python3-swsscommon_1.0.0_amd64.deb \
    /debs/sonic-db-cli_1.0.0_amd64.deb \
    /debs/libsaimetadata_1.0.0_amd64.deb \
    /debs/libsairedis_1.0.0_amd64.deb \
    /debs/libsaivs_1.0.0_amd64.deb \
    /debs/syncd-vs_1.0.0_amd64.deb \
    /debs/swss_1.0.0_amd64.deb

# Install debug symbols if needed
RUN if [ "$need_dbg" = "y" ] ; then dpkg -i /debs/libswsscommon-dbgsym_1.0.0_amd64.deb ; fi

# Copy the start script
COPY ["start.sh", "/usr/bin/"]

# Install Python and required Python packages
RUN pip3 install scapy==2.5.0

# Install additional dependencies required for networking tools and kernel module management
RUN apt-get -y install software-properties-common libdatetime-perl libcapture-tiny-perl build-essential libcpanel-json-xs-perl git python3-protobuf \
    && apt-get -y install kmod iproute2 net-tools

# Clean up unnecessary files to reduce image size
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
