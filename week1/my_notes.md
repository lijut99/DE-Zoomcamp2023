###To check if docker is properly installed

$ docker run -it hello-world

$ docker run -it ubuntu bash

$ docker run -it python:3.9.13


###For entrypoint as bash use below
docker run -it --entrypoint=bash python:3.9.13