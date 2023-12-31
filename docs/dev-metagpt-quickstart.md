---
title: "MetaGPT quickstart"
toc: true
top: true
date: 2023-07-16 08:00:00
updated: 2023-07-29 08:00:00
tags:
- python
- docker
- chatgpt
categories: 
- [engineering,python]
- [engineering,docker]
---

# Preface
Many friends who want to experience MetaGPT are stuck in the first step: installing and configuring the basic environment.
The goal of this article is to weaken the environmental restrictions and let everyone quickly experience the charm of MetaGPT. On the basis of not relying on any local environment (as long as you have a browser), it takes about 5 to 10 minutes for MetaGPT to start working.

Metagpt github repo：[geekan/MetaGPT](https://github.com/geekan/MetaGPT)

<!--more-->

# Step 1: Prepare the cloud environment
Instead of relying on the local environment, then we need a cloud environment. Here we choose to use [Docker Playground](https://labs.play-with-docker.com/), because this environment has Docker and can access https://api.openai.com/v1 at the same time, the environment is free.

Specific operation:

1. Visit [Docker Playground](https://labs.play-with-docker.com/), and log in with your docker hub account. If you don't have an account, register for one, it's free.

![](https://cdn.voidking.com/imgs/metagpt-quickstart/docker-playground.png?imageView2/0/w/800)

Note: Due to limited resources, sometimes the login may fail and the error message: We are really sorry but we are out of capacity and cannot create your session at the moment. Please try again later.

If the above prompt appears, you can wait for a while and log in again, or you can use [Kubernetes Playground](https://labs.play-with-k8s.com/) instead. Of course, if you have a more suitable cloud environment, it is also possible.

2. After logging in to Docker Playground, click `ADD NEW INSTANCE` to start a virtual machine.
![](https://cdn.voidking.com/imgs/metagpt-quickstart/add-instance.png?imageView2/0/w/800)


# Step 2: Prepare MetaGPT configuration
Refer to the Installation by Docker section of the document [MetaGPT README](https://github.com/geekan/MetaGPT/blob/main/README.md), and perform the following operations:

1. Download mirror
```bash
docker pull metagpt/metagpt:v0.3.1
```
![](https://cdn.voidking.com/imgs/metagpt-quickstart/docker-pull.png?imageView2/0/w/800)

2. Prepare to mount directory and configuration files
```bash
mkdir -p /opt/metagpt/{config,workspace}
docker run --rm metagpt/metagpt:v0.3.1 cat /app/metagpt/config/config.yaml > /opt/metagpt/config/key.yaml
vim /opt/metagpt/config/key.yaml # Change the config
```

![](https://cdn.voidking.com/imgs/metagpt-quickstart/config.png?imageView2/0/w/800)

编辑 key.yaml 时，如果使用的是OPENAI的API，那么只需要修改 `OPENAI_API_MODEL` 和 `OPENAI_API_KEY`，这两个值需要对应，`gpt-4`要对应 gpt-4 的KEY，`gpt-3.5-turbo`要对应 gpt-3.5 的KEY。

When editing key.yaml, if you are using the OPENAI API, you only need to modify `OPENAI_API_MODEL` and `OPENAI_API_KEY`, these two values need to correspond, `gpt-4` should correspond to the KEY of gpt-4, `gpt-3.5 -turbo` should correspond to the KEY of gpt-3.5.

# Step 3: Make MetaGPT work
1. Run a MetaGPT container
```bash
docker run --name metagpt -d \
    --privileged \
    -v /opt/metagpt/config/key.yaml:/app/metagpt/config/key.yaml \
    -v /opt/metagpt/workspace:/app/metagpt/workspace \
    metagpt/metagpt:v0.3.1
```
![](https://cdn.voidking.com/imgs/metagpt-quickstart/start-container.png?imageView2/0/w/800)

2. Enter the container to execute the command
```bash
docker exec -it metagpt /bin/bash
$ python startup.py "write a cli snake game"
```

![](https://cdn.voidking.com/imgs/metagpt-quickstart/metagpt-run.png?imageView2/0/w/800)

The files generated in the container will be stored in the directory `/app/metagpt/workspace`, which corresponds to the host directory `/opt/metagpt/workspace`.

# Step 4: Download the project
1. Package workspace
```bash
# Execute on the host machine
cd /opt/metagpt
tar -czvf workspace.tgz workspace
```

2. Start the web service to download the project
```bash
python -m http.server 9999
```

3. Docker Playground open port
Click `OPEN PORT` on the page, enter `9999`, and click OK to open the web page.

![](https://cdn.voidking.com/imgs/metagpt-quickstart/openport.png?imageView2/0/w/800)

4. Download project
On the web page, click workspace.tgz to download the project generated by MetaGTP.
![](https://cdn.voidking.com/imgs/metagpt-quickstart/download.png?imageView2/0/w/600)


