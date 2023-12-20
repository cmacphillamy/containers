The preferred way of managing software in nextflow dsl2 seems to be to use a separate container for each process. For example, for process `trim_galore`, use the `trim_galore` container. As such, we need to know how to create these containers.

While this may seem super annoying, it will simplify software dependencies considerably, as each image only needs to container one piece of software and its dependencies.

In a directory called `containers` or what ever you would like to call it, place a directory with the name of the software that will be in that container and within that directory, create `environment.yaml` and `Dockerfile` files. See below for an example:
```
mkdir filtlong
touch filtlong/environment.yaml
touch filtlong/Dockerfile
```

Open the `environment.yaml` and fill in the following:
```
name: filtlong # Unused in this example
channels:
- conda-forge
- bioconda
- defaults
dependencies:
- filtlong=0.2.1
```
The above will be what is installed into the container.

Open the `Dockerfile` and paste:
```
FROM mambaorg/micromamba:1.5.1 # Pulls the micromamba imag

COPY environment.yaml /tmp/environment.yaml # copies the environment.yaml into the container and stores it in /tmp/

# Installs the software in environment.yaml into the container
RUN micromamba install -y -n base -f /tmp/environment.yaml
RUN micromamba clean --all --yes
```
The `FROM` instruction pulls the `micromamba:1.5.1` image from the `mambaorg` repository. This means the container already has `mamba` installed, and we can go straight to installing the software we need.

Now that we have the environment set and the dockerfile ready, we can build the image.

```
docker build -t filtlong:v0.2.1 .
# OR
docker build -t callumm93/filtlong:v0.2.1 .
```

`-t` adds a `name:tag` to the image. Here, I used `filtlong` as that is the name of the software and `v0.2.1` as that is the version that was installed.
`.` means that docker will look in the current directory for the `Dockerfile`.

Once this has completed, you can run the container and confirm the software installed correctly with: `docker run -it filtlong:v0.2.1`.
`-it` allows you to interact with the container.

To then publish this to DockerHub for later use, we use: 
```
docker tag filtlong:v0.2.1 callumm93/filtlong:v0.2.1`

docker login

docker push callumm93/filtlong:v0.2.1
```
Where `callumm93` should be replaced with your DockerHub username.

We can then download the container and see if it works:
```
docker pull callumm93/filtlong:v0.2.1

docker run -it callumm93/filtlong:v0.2.1
```

To tidy up stopped containers, use: `docker container prune`
You can then remove the local image of the container with `docker images` followed by `docker rmi <image_id_to_remove>`. Then you can pull the image again and see if it works.