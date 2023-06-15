# nllab-Dockerfiles

Latest images on [DockerHub](https://hub.docker.com/):

	nadanai263/nllab-python:003 - Python 3.8
	nadanai263/nllab-jupyter:007 - Python 3.11.4, Julia 1.5.3
	nadanai263/nllab-julia:005 - Julia 1.5

The numeric tags will increase as the containers are updated, so please check them on [DockerHub](https://hub.docker.com/). 

### Running Linux in a Docker container

As an example to start an interactive Linux shell which contains a Python environment, first navigate to the directory containing your work. Then (making sure you have a working [Docker](https://www.docker.com) installation in place), run (on Mac, Linux)

	docker run -it --rm -v "$PWD":/app nadanai263/nllab-python:003 /bin/bash

On Windows, replace `"$PWD"` with `"%CD%"` (for command prompt) or `${pwd}` (for powershell).

This will pull and launch a Docker container, and start up a Linux shell. Your current working folder will be mounted to `/app`. Run your scripts as required.

### Running Jupyter notebook in a Docker container

You can directly start a Jupyter notebook in your current directory, using

	docker run -p 8888:8888 --rm -it -v "$PWD":/home/jovyan nadanai263/nllab-jupyter:007

Again, on Windows replace `"$PWD"` with `"%CD%"` (for command prompt) or `${pwd}` (for powershell).

---

### Useful Docker commands

#### 1. Building and pushing to registry

Building from a Dockerfile: navigate to the directory containing the file, which must be named `Dockerfile`, and run 

	docker build -t newrepo . # Create image using this directory's Dockerfile and name it 'newrepo'

To upload our image 'newrepo', first login to Dockerhub, then tag:

	docker login
	docker tag newrepo username/repository:tag

where `repository:tag` is the new repository name and version, e.g. `newrepo:001`, and `username` is your Dockerhub username. Finally, push:

	docker push username/repository:tag

#### 2. Running

An example of a run command in more detail: 

`docker run -p 8888:8888 --rm -it -v "$PWD":/home/jovyan/ username/repository:tag`

This command creates a Docker container from the online image, and launches it with the following options:
* `-p 8888:8888` exposes the container's internal ports
* `--rm` deletes the container and associated files at exit
* `-it` runs in interactive mode
* `-v "$PWD":/home/jovyan/` starts the container in your current directory
* `username/repository:tag` is the image:tag, corresponding to a specific image on DockerHub

#### 3. More examples of run commands

Mac/Linux versions given; as always on Windows, replace `"$PWD"` with `"%CD%"` (for command prompt) or `${pwd}` (for powershell).

To start a Linux shell interactively, mounting your current working directory to `/app`:

	docker run -it --rm -v "$PWD":/app nadanai263/nllab-python:003 /bin/bash

To start a Jupyter notebook:

	docker run -p 8888:8888 --rm -it -v "$PWD":/home/jovyan nadanai263/nllab-jupyter:007

To run a single python script called `script.py` located in your current directory:

	docker run --rm -v "$PWD":/app nadanai263/nllab-python:003 python app/script.py


#### 4. Maintenance and cleaning

To list all your local containers and images:

	docker container ls -a
	docker image ls -a

To remove a container, first run:

	docker container rm <containerID>

Then remove image:

	docker image rm <imageID>
