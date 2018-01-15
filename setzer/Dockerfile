FROM nixos/nix:1.11

RUN nix-channel --add https://nix.dapphub.com/pkgs/dapphub
RUN nix-channel --update
RUN nix-env -iA dapphub.seth dapphub.solc dapphub.dapp dapphub.jshon dapphub.bc dapphub.ethsign

RUN echo "@testing http://nl.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories
RUN apk update
RUN apk add --no-cache git make perl curl coreutils datamash@testing

RUN git clone https://github.com/makerdao/setzer

RUN cd setzer && make link

ENTRYPOINT [ "/bin/sh", "/usr/local/bin/setzer" ]
