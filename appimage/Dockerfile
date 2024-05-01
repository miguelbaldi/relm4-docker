FROM fedora:40
ARG RUST_VERSION=1.77.2
ENV RUST_VERSION=$RUST_VERSION

#RUN dnf groupinstall "C Development Tools and Libraries" -y
RUN dnf install rpm-build git gcc-c++ openssl-devel libgsasl-devel cmake gtk4-devel libadwaita-devel gtksourceview5-devel cyrus-sasl-devel cyrus-sasl cyrus-sasl-gssapi cyrus-sasl-plain -y

RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
RUN . ~/.cargo/env
RUN ls $HOME/.cargo/env
ENV PATH=/root/.cargo/bin:$PATH
RUN rustup install ${RUST_VERSION}


ENV APPIMAGE_VERSION=13
ENV APPIMAGE_EXTRACT_AND_RUN=1

RUN cargo install cargo-appimage

RUN dnf install wget -y

RUN wget https://github.com/AppImage/AppImageKit/releases/download/$APPIMAGE_VERSION/appimagetool-x86_64.AppImage
RUN chmod a+x appimagetool-x86_64.AppImage
RUN dd if=/dev/zero bs=1 count=3 seek=8 conv=notrunc of=appimagetool-x86_64.AppImage
RUN ls -lsha
RUN ./appimagetool-x86_64.AppImage --appimage-extract
RUN ls
RUN ln -nfs /squashfs-root/usr/bin/appimagetool /usr/bin/appimagetool

RUN dnf install file desktop-file-utils appstream -y

WORKDIR /mnt
RUN git config --global --add safe.directory /mnt
CMD ["/bin/bash"]
