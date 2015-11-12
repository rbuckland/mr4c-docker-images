# mr4c Docker Images

mr4c is a framework for running C/C++ libraries on an Hadoop Cluster using Yarn.

As mr4c is a C++ / Java Library, it requires some heavy-ish lifting to install on an OS. To simplify this process and make it easier to work with, I have created a set of Docker images that "wrap" up all the 
relevant library and compilation needs into a Docker image.

The idea is simply to "take" your Docker image to the cluster and launch mr4c from within the Container, onto the cluster.

Therefore working with mr4c, from within a docker container makes the setup and build of mr4c a lot nicer.

Because the C and C++ libraries are compiled to OS ones, and the actual Shared Libs are launched into the cluster, it then goes that the docker image that you use
has to match the hadoop cluster OS version. Of course, your mileage will vary if you try otherwise, but I can fair guess what the issues would be.

So with that in mind, the Docker Image has to be the OS equivalent to the nodes where your code executes.

If you are running an Hadoop Cluster that is NOT one of the ones I have developed, and feel so inclined as to build one; then create a Pull request to others may benefit.

I am sure the Centos 6 and the Ubuntu image provided are a VERY good starting base.

## Status

These are still a work in progress. (Ubuntu one is good)
If there is a ``Docker.public`` file, it just means it is re-ordered to what I want it to look like.
The ``Dockerfile`` is the _current_ working version.

## Your Code

Obviously the whole point of this is to compile your code with mr4c and launch it into Hadoop.
So you need to mount your code as a volume into the image (I use ``/opt/projects/``) within the container.
Adjust as required.

# Ubuntu Latest Image (15) 

CDH 5 Only supports up to 14, so I will create a 14 one shortly. Hopefully with Ubuntu it is simply a matter of changing the ``FROM`` tag and all packages behave as required.

```
cd mr4cdev-ubuntu
docker build -t mr4cdev-ubuntu .
docker run -ti --volume `pwd`/myprojects:/opt/projects  mr4cdev-ubuntu /bin/bash
```


# Centos 6 Image
This is the tricky one, Centos 6 is a little old so we do a lot of work to bring it up to scratch.

```
cd mr4cdev-centos
docker build -t mr4cdev-centos .
docker run -ti --volume `pwd`/myprojects:/opt/projects mr4cdev-centos /bin/bash
```

