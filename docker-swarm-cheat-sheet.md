### create a volume
```bash
docker volume create -d pxd --name testvol \
    --opt size=4 --opt block_size=64 --opt repl=3 --opt fs=ext4 --opt shared=true
```

### creating a service with a pre-existing volume
```bash
docker service create --name jenkins
--replicas 3 \
--publish 8082:8080
--publish 50000:50000
--mount “type=volume,volume-drive=pxd,source=jenkins_vol,target=/var/jenkins_home” \
--constraint ‘node.labels.jenkins_vol == true’ \
jenkins
```

### resize docker volume
`pxctl volume updatetestvol --size=30`

### take a cloud backup
`pxctl cloudsnap backup --volume testvol --full`

### dynamic volume creation
`docker run --volume-driver pxd -it -v io_priority=high,size=10G,repl=3 snap_schedule="periodic=60#4;daily=12:00#3",name=demovolume:/data busybox sh`