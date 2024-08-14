FROM ubuntu:latest
RUN apt-get -y update
RUN apt-get -y install texlive
RUN apt-get -y install ipe
RUN apt-get -y install texlive-fonts-extra
RUN apt-get -y install make
RUN apt-get -y install latexmk
RUN apt-get -y install pdftk-java
RUN apt-get -y install gnuplot
RUN apt-get -y install texlive-bibtex-extra
RUN apt-get -y install python3-pygments
# # just is not available via apt;  it wants snap which doesn't work in docker containers so we are manually downloading/running the just script into /usr/local
# RUN apt-get -y install curl
# RUN curl --proto '=https' --tlsv1.2 -sSf https://just.systems/install.sh | bash -s -- --to /usr/local/bin
USER ubuntu
