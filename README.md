# docker-nfs-server

## to start
    docker run -d --privileged --restart=always \
    -v /tmp:/nfs \
    -e NFS_EXPORT_DIR_1=/nfs \
    -e NFS_EXPORT_DOMAIN_1=\* \
    -e NFS_EXPORT_OPTIONS_1=ro,insecure,no_subtree_check,no_root_squash,fsid=1 \
    -p 111:111 -p 111:111/udp \
    -p 2049:2049 -p 2049:2049/udp \
    -p 32765:32765 -p 32765:32765/udp \
    -p 32766:32766 -p 32766:32766/udp \
    -p 32767:32767 -p 32767:32767/udp \
    fuzzle/docker-nfs-server:latest

## volumes
You will need to provide the container with the volume(s) that you want to expose via nfs
    
    -v <local path>:<path in container>

## environment variables
You will need to provide at the following 3 environment variables to configure the nfs exports:
* NFS_EXPORT_DIR_1
* NFS_EXPORT_DOMAIN_1
* NFS_EXPORT_OPTIONS_1

When the container is started, the environment variables are parsed and the following output is created in **/etc/exports** file:

    NFS_EXPORT_DIR_1 NFS_EXPORT_DOMAIN_1(NFS_EXPORT_OPTIONS_1)
for the example given the following line in **/etc/exports** would be created:

    /nfs *(ro,insecure,no_subtree_check, no_root_squash, fsid=1)

To define multiple exports, just increment the index on the environment variables

## build command
    docker build -t fuzzle/docker-nfs-server:v1 .

## inspect running container
docker exec -ti CONTAINER bash

## mounting the nfs share from another host
mount -v -t nfs -o ro,nfsvers=3,nolock,proto=udp,port=2049 <ip_address_docker_host>:/nfs /mnt/scratch

