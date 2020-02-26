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
## 2: Running *bet* within the container
---
layout: false

- running container
```bash
docker run --rm my_fsl
```
--

<img src="img/docker1.jpeg" width="95%" />

---
layout: false

- running command within the container
```bash
docker run --rm my_fsl bet
```
--

<img src="img/docker2.jpeg" width="95%" />


---
layout: false

- installing a datalad repository and downloading one T1w file

```bash
mkdir data
cd data
datalad install -r ///workshops/nih-2017/ds000114
datalad get ds000114/sub-01/ses-test/anat/sub-01_ses-test_T1w.nii.gz
cd ..
```

- if you don't have datalad yet, you can download

http://datasets.datalad.org/workshops/nih-2017/ds000114/sub-01/ses-test/anat/sub-01_ses-test_T1w.nii.gz

```bash
mkdir data
cd data
wget http://datasets.datalad.org/workshops/nih-2017/ds000114/sub-01/ses-test/anat/sub-01_ses-test_T1w.nii.gz
cd ..
```

---
layout: false

- mount a local directory with data (using read-only option) and running *bet* on the T1w file:
```bash
docker run -v /your/path/to/data:/data:/data:ro my_fsl bet \
/data/sub-01_ses-test_T1w.nii.gz sub-01_output
```
--
- checking the output
```bash
ls -l
```
--

<img src="img/docker3.jpeg" width="95%" />

---
layout: false

- creating a new directory for output
```bash
mkdir output
```

- mounting two local directories, with data and output, and running *bet* on the T1w file:
```bash
docker run -v /your/path/to/data:/data:ro -v ~/output:/output my_fsl bet \
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
--
- #### Automatically remove containers after stopping
  ```bash
  $ docker run --rm kaczmarj/coco2019
  ```

---
