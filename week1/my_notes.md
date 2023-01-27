##DE Zoom camp 2023 Notes


Root password of VM - happy


Setting up the Environment on Google Cloud


ssh -i ~/.ssh/gcp liju@35.240.120.86


Create a config file inside .ssh directory so that we don’t need use the external ip each time we have to connect to the server

$ touch config
$ code config ( this will open the config file in vs code where we can edit the details)

Host de-zoomcamp
  HostName 35.240.120.86
  User liju
  IdentityFile ~/.ssh/gcp


$ ssh de-zoomcamp ( now this command will directly connect us to the server)


After Anaconda is installed check bashrc  - 
$ less bashrc ( You will be able to see some update done by the anaconda installer )






Install docker

$ sudo apt-get install docker.io


Run docker as non sudo, followed the steps in this link - https://docs.docker.com/engine/install/linux-postinstall/




Install docker compose

Go to this link and download the latest release

Create a directory bin and download docker-compose in that

wget https://github.com/docker/compose/releases/download/v2.15.1/docker-compose-linux-x86_64 -O docker-compose

Make it executable - (base) liju@de-zoomcamp:~/bin$ chmod +x docker-compose


(base) liju@de-zoomcamp:~/bin$ ./docker-compose version
Docker Compose version v2.15.1



To execute docker-compose from any directory , add the PATH is .bashrc in the home directory –

export PATH="${HOME}/bin:${PATH}"



Validate

(base) liju@de-zoomcamp:~$ which docker-compose
/home/liju/bin/docker-compose
(base) liju@de-zoomcamp:~$ docker-compose version
Docker Compose version v2.15.1


Setup postgres and pgadmin , 

“cd” in to data-engineering-zoomcamp/week_1_basics_n_setup/2_docker_sql 


You will find a docker-compose.yml , it has the details of postgres and pgadmin, execute below command 

$ docker compose up -d



validate with docker ps

(base) liju@de-zoomcamp:~/data-engineering-zoomcamp/week_1_basics_n_setup/2_docker_sql$ docker ps
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                                            NAMES
f4a4c875926e   postgres:13      "docker-entrypoint.s…"   25 seconds ago   Up 18 seconds   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp        2_docker_sql-pgdatabase-1
2a4bb960a2bc   dpage/pgadmin4   "/entrypoint.sh"         25 seconds ago   Up 18 seconds   443/tcp, 0.0.0.0:8080->80/tcp, :::8080->80/tcp   2_docker_sql-pgadmin-1



Install pgcli 
$pip install pgcli


Validate installation 
$pgcli -h localhost -U root -d ny_taxi

Password for root is root , use ctrl+d to exit





Commands 

Docker
docker run -it ubuntu bash

docker run -it python:3.9.13


For entrypoint bash -  docker run -it --entrypoint=bash python:3.9.13







Build a image
Create a dockerfile with below content –
FROM python:3.9

RUN pip install pandas

ENTRYPOINT [ "bash" ]
~                     

Run the following command – 
liju@de-zoomcamp:~/test$ docker build -t test:pandas .
Start a container using the image that you just built –
docker run -it test:pandas









(base) liju@de-zoomcamp:~/data-engineering-zoomcamp$ docker --help build

Usage:  docker build [OPTIONS] PATH | URL | -

Build an image from a Dockerfile

Options:
      --add-host list           Add a custom host-to-IP mapping (host:ip)
      --build-arg list          Set build-time variables
      --cache-from strings      Images to consider as cache sources
      --cgroup-parent string    Optional parent cgroup for the container
      --compress                Compress the build context using gzip
      --cpu-period int          Limit the CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int           Limit the CPU CFS (Completely Fair Scheduler) quota
  -c, --cpu-shares int          CPU shares (relative weight)
      --cpuset-cpus string      CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string      MEMs in which to allow execution (0-3, 0,1)
      --disable-content-trust   Skip image verification (default true)
  -f, --file string             Name of the Dockerfile (Default is 'PATH/Dockerfile')
      --force-rm                Always remove intermediate containers
      --iidfile string          Write the image ID to the file
      --isolation string        Container isolation technology
      --label list              Set metadata for an image
  -m, --memory bytes            Memory limit
      --memory-swap bytes       Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --network string          Set the networking mode for the RUN instructions during build (default "default")
      --no-cache                Do not use cache when building the image
      --pull                    Always attempt to pull a newer version of the image
  -q, --quiet                   Suppress the build output and print image ID on success
      --rm                      Remove intermediate containers after a successful build (default true)
      --security-opt strings    Security options
      --shm-size bytes          Size of /dev/shm
  -t, --tag list                Name and optionally a tag in the 'name:tag' format
      --target string           Set the target build stage to build.
      --ulimit ulimit           Ulimit options (default [])





Answers

--iidfile string          Write the image ID to the file



(base) liju@de-zoomcamp:~/data-engineering-zoomcamp$ docker run -it --entrypoint=bash python:3.9
root@65a747216f03:/# pip list
Package    Version
---------- -------
pip        22.0.4
setuptools 58.1.0
wheel      0.38.4
WARNING: You are using pip version 22.0.4; however, version 22.3.1 is available.
You should consider upgrading via the '/usr/local/bin/python -m pip install --upgrade pip' command.


Answer 

Run docker with the python:3.9 image in an interactive mode and the entrypoint of bash. Now check the python modules that are installed ( use pip list). How many python packages/modules are installed?

3










Run postgres

docker run -it \
    -e POSTGRES_USER="root" \
    -e POSTGRES_PASSWORD="root" \
    -e POSTGRES_DB="ny_taxi" \
    -v $(pwd)/data/ny_taxi_postgres_data:/var/lib/postgresql/data \
    -p 5432:5432 \
    --network=pg-network \
    --name=pg-database \
    postgres:13

Run pgAdmin


/home/liju/mygit/DE-Zoomcamp2023/week1/data/ ny_taxi_postgres_data


###To check if docker is properly installed

$ docker run -it hello-world

$ docker run -it ubuntu bash

$ docker run -it python:3.9.13


###For entrypoint as bash use below
docker run -it --entrypoint=bash python:3.9.13