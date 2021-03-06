# Docker for A.I. Researcher

![Docker Build Status](https://img.shields.io/docker/build/eungbean/deepo)
![Docker Automated build](https://img.shields.io/docker/automated/eungbean/deepo)
![Docker Pulls](https://img.shields.io/docker/pulls/eungbean/deepo)
![GitHub](https://img.shields.io/github/license/eungbean/Docker-for-AI-Researcher)

**"Docker for A.I. Researcher"** is a series of Shell script that  
_ allows you to quickly set up your deep learning research environment  
_ supports almost [all commonly used deep learning frameworks](https://github.com/eungbean/Docker-for-AI-Researcher#Available-tags) with GPU acceleration (CUDA and cuDNN included)  
_ supports the next-generation web-based user interface IDE, [Jupyter-lab](https://jupyterlab.readthedocs.io/en/stable/)  
_ supports remote work with laptop OUTSIDE of the lab  
 \* includes fancy terminal setup with oh-my-zsh.

# Demo

[Jupyterlab Docker: Try it on Binder](https://mybinder.org/v2/gh/jupyterlab/jupyterlab-demo/master?urlpath=lab/tree/demo)

---
# 1. Requirements

- Nvidia GPU Driver Installation
- 10 minuites
---
# 2. Quick Start
### step 1. clone the repository

```sh
sudo apt-get install git
git clone https://github.com/eungbean/Docker-for-AI-Researcher
cd Docker-for-AI-Researcher
```

---
### Setp 2. Set your arguments

```sh
./SETTING.sh
```

* Set your SSH Password to `SETUP_DOCKER_VSCODE='root'`
* To install VScode Containier, You need to set `SETUP_DOCKER_VSCODE=true` in `SETTINGS.sh`.

- You need to configure variables with `# Required` tag.
- Boolean (ex: `true`) are not supported. Use strings instead. (ex: `\'true\'`)
- Followings are some examples.

```sh
##############################
SET_1_TERMINAL_SETTING='false'
SET_2_DOCKER_INSTALL='false'
SET_3_VSCODE_CONTAINER_BUILD='true'
SET_4_VSCODE_CONTAINER_RUN='true'
SET_5_SETTING_ALIAS='false'
##############################

# DOCKER-VSCODE SETTING
SETUP_DOCKER_VSCODE='true'   #REQUIRED
VS_PASSWORD='root'         #REQUIRED
...
```

---

### Step 3. BOOM!

```sh
./INSTALL.sh
```

that's it. all set!  
Grab some coffee for 10 minuites!

---

### Step 4. Post Installation

#### 1) Send ssh key to container
```sh
ssh-copy-id -i ~/.ssh/id_rsa -p 10022 root@your.ip.add.ress
```


#### 2) Get inside docker container
* with ssh
```sh
ssh -p 10022 root@your.ip.add.ress
```

Initial ssh id/pw is `root/root`.

* docker exec
```sh
docker exec -it ${CONTAINER_NAME} /usr/bin/zsh
```

#### 3) Change ssh password
```
(inside docker container)
passwd
```
You have to set your own password.


#### 4) Run `code-server` on background.
```sh
(inside docker container)
nohup code-server --bind-addr 0.0.0.0:8080 . &!
```

Now if you access http://your.ip.add.ress:18080, you should see code-server!
Check the config file at `SETTINGS.sh` for the password.



### Step 5. Let's use it!

- VScode Container  
  **VSCODE**: `http://your.ip.addr.ess:18080`  
  **Tensorboard**: `http://your.ip.addr.ess:16006`
  **SSH**: `ssh -p 10022 root@your.ip.addr.ess`

Without any configuration, initial password is `root`.

- Jupyter Container  
  **Jupyter**: `http://your.ip.addr.ess:28000`  
  **Tensorboard**: `http://your.ip.addr.ess:26006`  
  **SSH**: `ssh -p 20022 root@your.ip.addr.ess`

Without any configuration, initial password is `root`.

---
# 3. More Detailed..

#### `1-terminal_setting.sh`

```sh
sudo sh ./01-terminal-setting.sh
```

Install following packages..

- Open SSH
- Terminal tools
  - zsh
  - oh-my-zsh
  - zsh-syntax-highlighting
  - zsh-autosuggestions
  - neovim
  - spacevim
  - powerline font
- GPU Monitoring tools
  - gpustat
  - glances
- git

---

#### `2-docker_setup.sh`

```sh
sudo sh ./02-docker-setup.sh
```

> Install followings..

- [Docker-ce](https://docs.docker.com/install/linux/docker-ce/ubuntu)
- [nvidia-docker](https://github.com/NVIDIA/nvidia-docker)

If the installation is done, the message will be displayed.

```sh
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
...
```

---

#### `3-build_docker_jupyter.sh` / `3-build_docker_vscode.sh`

I strongly recommend to use [ufoym/deepo](https://github.com/ufoym/deepo) image from scratch.  
This image supports almost all commonly used deep learning frameworks.

```sh
sudo docker pull ufoym/deepo:all-jupyter
```

---

In addition to ufoym/deepo image, I made my own docker image called [eungbean/deepo:lab]().
This image includes more useful packages to start with.
It will reduce your time to set up initial research environment.  
Trust me, you'll happy with it.

```sh
sudo docker pull ufoym/deepo:all-jupyter
```

---

#### `4-run_docker_jupyter.sh` / `4-run_docker_vscode.sh`

deploy your container.

---

#### `5-setting_alias.sh`

Setup some aliases for convinience.
