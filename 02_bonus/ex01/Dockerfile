FROM ubuntu

RUN apt-get update && \
    apt-get upgrade -y &&\
	apt-get install -y cowsay &&\
	rm -rf /var/lib/apt/lists/*

ENV PATH $PATH:/usr/games

COPY docker.cow /usr/share/cowsay/cows/docker.cow
#RUN ln -sv /usr/share/cowsay/cows/docker.cow /usr/share/cowsay/cows/default.cow


ENTRYPOINT ["/usr/games/cowsay","-f","docker"]
CMD ["moby","dock"]
