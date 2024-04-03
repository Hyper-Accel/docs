
## Deploy with Docker
If you get the server from HyperDex Engineers, there will be HyperDex-Docker Container named hrt-docker.

```bash
 hyunjun@ha-server:~$ docker ps -a
 CONTAINER ID   IMAGE                               COMMAND       CREATED         STATUS                      PORTS     NAMES
 a1236017d764   hyperaccel/hrt:1.3.1-ubuntu-20.04   "/bin/bash"   9 seconds ago   Up 8 seconds                          hrt-docker
```

Before dive into the docker, you have to get some script to run LPU.
```bash
docker cp text_generation.py hrt-docker:/root/.
```

Enter the docker,
```bash
docker exec -it hrt-docker /bin/bash
```

then you can use LPU.
```bash
python text_generation.py
```




