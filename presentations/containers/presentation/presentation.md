name: inverse
layout: true
class: center, middle, inverse
---
# Container-based environments

---

name: inverse
layout: true
class: center, middle, inverse

---
## Introduction

---

layout: false


### <span style="color:purple">Common scenarios in research</span>

- #### Science these days rely extensively on software
- #### Scientist don't have computational background
- #### Computational environments might be difficult to create (and even harder to reproduce!)
- #### Software changes (unknowingly) during the life of a study
---

### <span style="color:purple">What is a container?</span>

#### A container is a "standardized unit of software" that can run anywhere

### <span style="color:purple">What do containers provide?</span>

- #### Standard method of creating and sharing computational environments
- #### Isolation of computational environments
- #### Easy interoperability
  - Containers can be run on Linux, macOS, and Windows
- #### Immutability of environments
  - You cannot permanently alter a container unintentionally

---

### <span style="color:purple">Why do we need containers?</span>
#### <span style="color:purple">Long term perspective</span>

### Science Reproducibility

  - Each project in a lab depends on complex software environments
    - operating system
    - drivers
    - software dependencies: Python/MATLAB/R + libraries
&nbsp;


  - Containers:
    - allow to encapsulate your environment
    - you (and others!) can recreate the environment later in time

---
### <span style="color:purple"> Why do we need containers?</span>
#### <span style="color:purple"> Short term perspective</span>

### Collaboration with your colleagues

- Sharing your code or using a repository might not be enough
&nbsp;


- Containers:
  - encapsulate your environment
  - you can easily share the environment


---
### <span style="color:purple"> Why do we need containers?</span>
#### <span style="color:purple"> Short term perspective</span>

### Changing hardware

- The personal laptop might not be enough at some point
&nbsp;


- Containers:
  - encapsulate your environment
  - you can easily recreate the environment on a different machine


---

###<span style="color:purple">Why do we need containers?</span>
#### <span style="color:purple">Short term perspective</span>

### Freedom to experiment!
- Universal Install Script from xkcd: *The failures usually don’t hurt anything...*
 And usually all your old programs work...

<img src="img/universal_install_script_2x.png" width="40%" />

---
### <span style="color:purple"> Why do we need containers?</span>
#### <span style="color:purple"> Short term perspective</span>

### Using existing environments


- Installing all dependencies is not always easy.
&nbsp;


- Containers:
  - isolate and encapsulate the environments
  - there are many ready to use existing environments (check [Docker Hub](https://hub.docker.com/))

---

name: inverse
layout: true
class: center, middle, inverse
---
## Tools
---
layout: false

### <span style="color:purple">Virtual Machines and Containers</span>

- Two main types:

  - Virtual Machines:

      - Virtualbox
      - VMware
      - AWS, Google Compute Engine, ...

  - Containers:

      - Docker
      - Singularity
&nbsp;

--

- Main idea -- isolate the computing environment

  - Allow regenerating computing environments
  - Allow sharing your computing environments
&nbsp;

--

**Containers are very lightweight and faster to start up 
comparing to standard Virtual Machines**

---
###<span style="color:purple">Docker and Singularity </span>
- **Docker:**
  - leading software container platform
  - an open-source project
  - **it runs on Linux, macOS, and Windows Pro** (you don't have to install VM!)
--

  - **can escalate privileges - not supported by HPC centers admins**

--

- **Singularity:**
  - a container solution created for scientific application
  - **supports existing and traditional HPC resources**
  - a user inside a Singularity container is the same user as outside the container
(so you can be a root only if you were root on the host system)
  - VM needed on Windows and OSX (some support for OSX, beta release)
  - a Singularity image can be created from a Docker image

---

name: inverse
layout: true
class: center, middle, inverse
---
## Exercises
---
layout: false

name: inverse
layout: true
class: center, middle, inverse
---
### 1. Use Docker command-line interface
---
layout: false

- #### Show Docker help
  ```bash
  $ docker --help
  ```
--
- #### List Docker images
  ```bash
  $ docker images
  ```
--
- #### Pull a Docker image
  ```bash
  $ docker pull hello-world
  ```
--
- #### List Docker images again
  ```bash
  $ docker images
  ```

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              fce289e99eb9        9 days ago          1.84kB
```


---
name: inverse
layout: true
class: center, middle, inverse
---
## 2: Using *my_fsl* container to run *bet*
---
layout: false

### <span style="color:purple">What does it mean to work in a container</span>


If you are running a container on your laptop
&nbsp;

- it uses the same hardware

- but user spaces and libraries are independent

--

<img src="img/docker1in.jpeg" width="20%" />


<img src="img/docker2in.jpeg" width="50%" />

---
### <span style="color:purple">What does it mean to work in a container</span>


If you are running a container on your laptop
&nbsp;

- it uses the same hardware

- but user spaces and libraries are independent

- you can create additional bindings between these two environments

<img src="img/docker3in.jpeg" width="70%" />

---


- running container
```bash
docker run --rm my_fsl
```
--

<img src="img/docker1.jpeg" width="95%" />

---
layout: false

- running *bet* command within the container
(*bet* - Brain Extraction Tool)
```bash
docker run --rm my_fsl bet
```
--

<img src="img/docker2.jpeg" width="95%" />


---
layout: false

- creating a new directory for output
```bash
mkdir output
```

- mounting two local directories, with data and output, and running *bet* on the T1w file:
```bash
docker run -v /home/ubuntu/containers_lesson/data:/data:ro -v /home/ubuntu/containers_lesson/output:/output my_fsl bet \
/data/sub-01_ses-test_T1w.nii.gz /output/sub-01_output
```
--
- checking the output
```bash
ls -l output
```
--

<img src="img/docker4.jpeg" width="90%" />

---
layout: false

we can also run a script with the container

- creating a new directory
```bash
mkdir scripts
```

- creating a script (use any editor you want)
```bash
emacs scripts/fsl_bash.sh &
```

- you can run your favourite FSL command
```bash
#!/bin/bash
bet "$1" "$2" -m
```

- mounting three local directories, with data, with output and with the script, and running *bet* on the T1w file:
```bash
docker run -v /home/ubuntu/containers_lesson/data:/data:ro \
-v /home/ubuntu/containers_lesson/output:/output \
-v /home/ubuntu/containers_lesson/scripts:/scripts \
my_fsl bash /scripts/fsl_bash.sh \
/data/sub-01_ses-test_T1w.nii.gz /output/sub-01_output
```

---
layout: false

### Running `bet` analysis using singularity

```bash
$ singularity run /home/ubuntu/containers_lesson/images/my_fsl_sing.img bet \
/home/ubuntu/containers_lesson/data/sub-01_ses-test_T1w.nii.gz \
/home/ubuntu/containers_lesson/output/sub-01_output_sing
```

- checking the output

```bash
ls -l ~/output
```

---
layout: false

---

name: inverse
layout: true
class: center, middle, inverse
---
### Creating an image with FSL
---
layout: false

- #### In order to create a docker image we need to write a Dockerfile, that contains all the commands a user could call on the command line to assemble an image. Dockerfile provide a “recipe” for an image.

  - a simple Dockerfile might look like this

  ```bash
  FROM ubuntu:latest
  RUN apt-get update -y && apt-get install -y git emacs
  ```

---
layout: false
  - a more complicated Dockerfile with FSL might look like this

  ```dockerfile
  FROM neurodebian:stretch-non-free
  ARG DEBIAN_FRONTEND="noninteractive"

  ENV LANG="en_US.UTF-8" \
      LC_ALL="en_US.UTF-8" \
      ND_ENTRYPOINT="/neurodocker/startup.sh"
  RUN export ND_ENTRYPOINT="/neurodocker/startup.sh" \
      && apt-get update -qq \
      && apt-get install -y -q --no-install-recommends \
             apt-utils bzip2 ca-certificates \
             curl locales unzip \
      && apt-get clean \
      && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
      && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
      && dpkg-reconfigure --frontend=noninteractive locales \
      && update-locale LANG="en_US.UTF-8" \
      && chmod 777 /opt && chmod a+s /opt \
      && mkdir -p /neurodocker \
      && if [ ! -f "$ND_ENTRYPOINT" ]; then \
           echo '#!/usr/bin/env bash' >> "$ND_ENTRYPOINT" \
      &&   echo 'set -e' >> "$ND_ENTRYPOINT" \
      &&   echo 'if [ -n "$1" ]; then "$@"; else /usr/bin/env bash; fi' >> "$ND_ENTRYPOINT"; \
      fi \
      && chmod -R 777 /neurodocker && chmod a+s /neurodocker
  ENTRYPOINT ["/neurodocker/startup.sh"]
  RUN apt-get update -qq \
      && apt-get install -y -q --no-install-recommends \
             fsl-5.0-core \
             fsl-mni152-templates \
      && apt-get clean \
      && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
  RUN sed -i '$isource /etc/fsl/5.0/fsl.sh' $ND_ENTRYPOINT

  ```

---
layout: false

### [Neurodocker](https://github.com/repronim/neurodocker)

&nbsp;
- generates custom  Dockerfiles and Singularity files for neuroimaging software and minifies existing Docker images

- simplifies writing Dockerfiles and Singularity files

- incorporates the best practice for installing software

- supports popular neuroimaging software: AFNI, ANTs, Convert3D, dcm2niix, FreeSurfer, FSL, MINC, Miniconda (Python), MRtrix3, PETPVC, SPM, NeuroDebian

- uses ReproZip for minifying existing Docker images

--

- Creating a Dockerfile that includes FSL from Neurodebian:

  ```bash
  docker run --rm kaczmarj/neurodocker:0.6.0 generate docker \
  --base neurodebian:stretch-non-free \
  --pkg-manager apt \
  --install fsl-5.0-core fsl-mni152-templates \
  --add-to-entrypoint "source /etc/fsl/5.0/fsl.sh"
  ```
---
layout: false

### Building a Docker image

- creating a new empty directory 
```bash
mkdir my_docker
cd my_docker
```

- create a Dockerfile using Neurodocker:
```bash
docker run --rm kaczmarj/neurodocker:0.6.0 generate docker \
--base neurodebian:stretch-non-free \
--pkg-manager apt \
--install fsl-5.0-core fsl-mni152-templates \
--add-to-entrypoint "source /etc/fsl/5.0/fsl.sh" > Dockerfile 
```

- build a Docker image: 
```bash
docker build -t my_new_fsl . 
```

- check available Docker images: 
```bash
docker images
```

---
layout: false

### Building a Singularity image

- creating a Singularity file using Neurodocker:
```bash
docker run --rm kaczmarj/neurodocker:0.6.0 generate singularity \
--base neurodebian:stretch-non-free \
--pkg-manager apt \
--install fsl-5.0-core fsl-mni152-templates \
--add-to-entrypoint "source /etc/fsl/5.0/fsl.sh" > Singularity_fsl 
```

- building a Singularity image: 
```bash
sudo singularity build my_new_fsl_sing.img Singularity_fsl 
```

---
layout: false

### Using existing containers

- check the [Docker Hub](https://hub.docker.com/)

---


### Cleaning up after yourself

- #### Show all containers
  ```bash
  $ docker ps -a
  ```
--
- #### Remove all stopped containers
  ```bash
  $ docker container prune
  ```
