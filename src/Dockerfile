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
