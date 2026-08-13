#pull from arch docker container
FROM archlinux:latest
LABEL maintainer="GarretSidzaka <garretsidzaka@proton.me>" 

# Update package repos hxxps://github.com/Steam-Headless/docker-steam-headless/blob/master/Dockerfile.arch
RUN \
    echo "**** Update package manager ****" \
        && sed -i 's/^NoProgressBar/#NoProgressBar/g' /etc/pacman.conf \
        && echo -e "[multilib]\nInclude = /etc/pacman.d/mirrorlist" >> /etc/pacman.conf \
    && \
    echo

# Update locale
RUN \
    echo "**** Configure locals ****" \
        && echo 'en_US.UTF-8 UTF-8' > /etc/locale.gen \
        && locale-gen \
    && \
    echo
ENV \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=en_US.UTF-8
# Re-install certificates
RUN \
    echo "**** Install certificates ****" \
	    && pacman -Syu --noconfirm --needed \
            ca-certificates \
    && \
    echo "**** Section cleanup ****" \
	    && pacman -Scc --noconfirm \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
    && \
    echo


# Install core packages
RUN \
    echo "**** Install tools ****" \
	    && pacman -Syu --noconfirm --needed \
            base-devel \
            bash \
            bash-completion \
            btop \
            curl \
            docbook-xml \
            docbook-xsl \
            fakeroot \
            p7zip \
            patch \
	    gcc \ 
            git \
	    gzip \
            jq \
	    libx11 \
            locate \
	    make \
            man-db \
            nano \
	    nasm \
            networkmanager \
            net-tools \
	    openssh \
	    pam \
            pciutils \
            pkg-config \
            procps \
            psmisc \
            psutils \
            rsync \
            screen \
            sudo \
            tar \
 	    unzip \
            vim \
            wget \
            xmlstarlet \
            xz \
    && \
    echo "**** Install python ****" \
	    && pacman -Syu --noconfirm --needed \
            python \
            python-numpy \
            python-pip \
            python-setuptools \
    && \
    echo "**** Section cleanup ****" \
	    && pacman -Scc --noconfirm \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
    && \
    echo

# Configure default user and set user env
ENV \
    PUID=99 \
    PGID=100 \
    UMASK=000 \
    USER="default" \
    USER_PASSWORD="agoodpassword" \
    USER_HOME="/home/default" \
    TZ="America/Los_Angeles" \
    USER_LOCALES="en_US.UTF-8 UTF-8"
RUN \
    echo "**** Configure default user '${USER}' ****" \
        && mkdir -p \
            ${USER_HOME} \
        && useradd -d ${USER_HOME} -s /bin/bash ${USER} \
        && chown -R ${USER} \
            ${USER_HOME} \
        && echo "${USER} ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers \
    && \
    echo

# Install yay
RUN \
    echo "**** Install Yay ****" \
	    && pacman -Sy \
	    && su - default -c 'git clone https://aur.archlinux.org/yay.git /tmp/yay && cd /tmp/yay && makepkg --noconfirm --syncdeps --install' \
    && \
    echo "**** Section cleanup ****" \
	    && pacman -Scc --noconfirm \
        && rm -fr /home/default/.cache/yay \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
        && rm -rf /tmp/yay* \
    && \
    echo

# Install supervisor
RUN \
    echo "**** Install supervisor ****" \
	    && pacman -Syu --noconfirm --needed \
            supervisor \
    && \
    echo "**** Section cleanup ****" \
	    && pacman -Scc --noconfirm \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
    && \
    echo

# Install mscorefonts
RUN \
    echo "**** Install ms fonts ****" \
        && su - default -c "yay -Syu --noconfirm --needed ttf-ms-fonts" \
    && \
    echo "**** Section cleanup ****" \
	    && pacman -Scc --noconfirm \
        && rm -fr /home/default/.cache/yay \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
        && rm -rf /tmp/yay* \
    && \
    echo


# Install X Server requirements
ENV \
    XORG_SOCKET_DIR="/tmp/.X11-unix" \
    XDG_RUNTIME_DIR="/tmp/.X11-unix/run" \
    XDG_SESSION_TYPE="x11" \
    FORCE_X11_DUMMY_CONFIG="false"
RUN \
    echo "**** Install X Server requirements ****" \
	    && pacman -Syu --noconfirm --needed \
            avahi \
            dbus \
            lib32-fontconfig \
            fuse2 \
            x11vnc \
            xorg \
            xorg-apps \
            xorg-font-util \
            xorg-fonts-misc \
            xorg-fonts-type1 \
            xorg-server \
            xorg-server-xephyr \
            xorg-server-xvfb \
            xorg-xauth \
            xorg-xbacklight \
            xorg-xhost \
            xorg-xinit \
            xorg-xinput \
            xorg-xkill \
            xorg-xprop \
            xorg-xrandr \
            xorg-xsetroot \
            xorg-xwininfo \
            xf86-input-evdev \
            xterm \
    && \
    echo "**** Section cleanup ****" \
	    && pacman -Scc --noconfirm \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
    && \
    echo

# Install audio requirements
ENV \
    PULSE_SOCKET_DIR="/tmp/pulse" \
    PULSE_SERVER="unix:/tmp/pulse/pulse-socket"
RUN \
    echo "**** Install X Server requirements ****" \
	    && pacman -Syu --noconfirm --needed \
            alsa-utils \
            pavucontrol \
            pulseaudio \
            pulseaudio-alsa \
    && \
    echo "**** Section cleanup ****" \
	    && pacman -Scc --noconfirm \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
    && \
    echo

# Install desktop environment
# TODO: Install equivelent of 'msttcorefonts' and 'fonts-vlgothic'
RUN \
    echo "**** Install desktop environment ****" \
	    && pacman -Syu --noconfirm --needed \
            gedit \
            xfce4 \
            xfce4-goodies \
            xfce4-terminal \
            ttf-liberation \
            xdg-user-dirs \
            xdg-utils \
            imagemagick \
    && \
    echo "**** Section cleanup ****" \
	    && pacman -Scc --noconfirm \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
    && \
    echo

# Add support for flatpaks
ENV \
    XDG_DATA_DIRS="/home/default/.local/share/flatpak/exports/share:/var/lib/flatpak/exports/share:/usr/local/share/:/usr/share/"
RUN \
    echo "**** Install flatpak support ****" \
	    && pacman -Syu --noconfirm --needed \
            flatpak \
            xdg-desktop-portal-gtk \
            gnome-software \
    && \
    echo "**** Configure flatpak ****" \
        && chmod u-s /usr/bin/bwrap \
        && flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo \
        && su - default -c "flatpak remote-add --if-not-exists --user flathub https://flathub.org/repo/flathub.flatpakrepo" \
    && \
    echo "**** Section cleanup ****" \
	&& pacman -Scc --noconfirm \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
    && \
    echo

# Install Steam
RUN \
    echo "**** Install Steam ****" \
	    && pacman -Syu --noconfirm --needed \
            steam \
    && \
    echo "**** Section cleanup ****" \
	    && pacman -Scc --noconfirm \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
    && \
    echo

# Various other tools
RUN \
    echo "**** Install dumb-init ****" \
        && wget --no-check-certificate --no-cookies --quiet \
            -O /usr/bin/dumb-init \
            https://github.com/Yelp/dumb-init/releases/download/v1.2.5/dumb-init_1.2.5_x86_64 \
        && chmod +x /usr/bin/dumb-init \
    && \
    echo "**** Install dumb-udev ****" \
        && python3 -m pip install \
            --break-system-packages \
            --pre \
            --upgrade \
            --no-cache-dir \
            git+https://github.com/Steam-Headless/dumb-udev.git@1.0.2 \
    && \
    echo

# Install Protonup replacement
RUN \
    echo "**** Install PorotonUP ****" \
        && su - default -c "yay -Sy --noconfirm --needed --overwrite \"/usr/bin/normalizer\" protonup-qt" \
    && \
    echo "**** Section cleanup ****" \
	    && pacman -Scc --noconfirm \
        && rm -fr /home/default/.cache/yay \
        && rm -fr /var/lib/pacman/sync/* \
        && rm -fr /var/cache/pacman/pkg/* \
        && rm -rf /tmp/yay* \
    && \
    echo

# Install xrdp
RUN \
    echo "**** Install xRDP from src ****" \
		&& mkdir -p /tmp/xrdp \
		&& cd /tmp/xrdp \
		&& wget --no-check-certificate --no-cookies -qO- https://github.com/neutrinolabs/xrdp/releases/download/v0.10.6.1/xrdp-0.10.6.1.tar.gz | gunzip | tar xvf - \
		&& cd /tmp/xrdp/xrdp-0.10.6.1 \
		&& ./bootstrap \
		&& ./configure \
		&& make \
		&& make install \
	&& \
	echo "**** Build cleanup ****" \
		&& rm -fr /tmp/xrdp/* \
	&& \
	echo
	


# Add FS overlay
COPY overlay /


# Set display environment variables
ENV \
    DISPLAY_CDEPTH="24" \
    DISPLAY_REFRESH="120" \
    DISPLAY_SIZEH="900" \
    DISPLAY_SIZEW="1600" \
    DISPLAY_VIDEO_PORT="DFP" \
    DISPLAY=":55"

# Expose the required ports
EXPOSE 3389
EXPOSE 22


# Use dumb-init as the entry point
ENTRYPOINT ["/usr/local/bin/dumb-init", "--"]

# Start the VNC server
CMD ["/entrypoint"]
