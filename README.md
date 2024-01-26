# Precimonious-Clang-Plugins

### 1. Setup

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

##### Check if you are in the docker container

```
root@<container_id>:~/home# ls
Precimonious  README.md
root@<container_id>:~/home# pwd
/root/home
```


### 2. Run Precimonious on NAS CG

Run the following commands. (approx. 1h) 
`cg` specifies the name of the benchmark, 
and `10` indicates the timeout in seconds to run the benchmark `cg`.

```
cd /root/home/Precimonious
python3 run.py cg 10
```
Expected results:

After the precision tuning is done, you can find a folder in `/root/home/Precimonious/cg/run/results-eps==4-A` which contains the following files:

- `*.json`: all precision configurations in the search
- `.log`: a log file containing model prediction results for each configuration and the corresponding verification results
- `dd2_valid_{BENCH}_{IDX}.json`: the best precision configuration found by our tool
- `best_speedup_{BENCH}_{IDX}.txt`: the corresponding best speed up


### 3. How to Use Clang Plugins Separately

There are two Clang plugins [`CreateSearchSpace.so`](https://github.com/ucd-plse/Precimonious-Clang-Plugins/blob/main/Precimonious/plugin/CreateSearchSpace.so) and 
[`TransformType.so`](https://github.com/ucd-plse/Precimonious-Clang-Plugins/blob/main/Precimonious/plugin/TransformType.so) which can be found in 
`Precimonious-Clang-Plugins/Precimonious/plugin/`.
`CreateSearchSpace.so` is used to create the search space
of a benchmark and `TransformType.so` is to transform a benchmark
based on a specific precision configuration.


#### Compile Clang Plugins

First, you need to compile the clang plugins. The repository contains the clang plugin codes [`CreateSearchSpace.cpp`](https://github.com/ucd-plse/Precimonious-Clang-Plugins/blob/main/Precimonious/plugin/CreateSearchSpace.cpp) and [`TransformType.cpp`](https://github.com/ucd-plse/Precimonious-Clang-Plugins/blob/main/Precimonious/plugin/TransformType.cpp).  The repository already has compiled clang plugins `CreateSearchSpace.so` and 
`TransformType.so`. To compile them mannually, run the following commands.

```
cd /root/home/Precimonious/plugin
make PLUGIN=CreateSearchSpace
make PLUGIN=TransformType
```

#### Use Clang Plugins on NAS CG

1. Generate search space of `cg`

```
cd /root/home/Precimonious/cg/run
python3 generate-include.py
python3 setup.py cg
python3 create-search-space.py cg
```

2. Apply a precision configuration on `cg`


```
python3 trans-type.py example.json cg
```

