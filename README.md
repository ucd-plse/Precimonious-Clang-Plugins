# Precimonious-Clang-Plugins

### Setup

#### Required Prerequisites

Hardware:

- 40GB free disk space recommended (At lease more than 30GB to download the docker image and the github repo.)

Software:

- Ubuntu 
    - Recommended version: 20.04 with kernel version 5.14.0 (The reproduction package has not been tested on other operating systems.)
- Docker 
    - Recommended version: 23.0.1 (The reproduction package has not been tested with other Docker versions.)

#### Steps to Set Up

In the following steps, please replace `<YOUR LOCAL PATH TO THIS REPO>`
to your local path of this github repository.

##### Step 1: Git clone the repo (approx. a few secs)

Clone this GitHub repository to your local directory.

```
git clone https://github.com/ucd-plse/Precimonious-Clang-Plugins.git <YOUR LOCAL PATH TO THIS REPO>
```

##### Step 2: Pull the Docker image (approx. 15min)

Please note that the docker image size is 28.3GB.

```
docker pull ucdavisplse/fplearner
```


##### Step 3: Run a Docker container (approx. a few secs)


```
docker run -v <YOUR LOCAL PATH TO THIS REPO>:/root/home -ti  --name precimonious ucdavisplse/fplearner
```


If necessary, you can also change the container's name.

In the subsequent process of experiments reproduction,
the first step is always to make sure you are inside the docker container. If you are not, please run the following command (approx. a few secs):

```
docker start -i precimonious
```
To exit and stop the container, press Ctrl+D.


### Run Precimonious on NAS CG

Run the following commands. (approx. 1h) 
`cg` specifies the name of the benchmark, 
and `10` indicates the timeout in seconds to run the benchmark `cg`.

```
cd /root/home/Precimonious
python3 run.py cg 10
```

### Compile and Use Clang Plugins

Compile clang plugins.

```
cd /root/home/Precimonious/plugin
make PLUGIN=CreateSearchSpace
make PLUGIN=TransformType
```

