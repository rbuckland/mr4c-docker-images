# Building Docker Image

Working with mr4c, from within a docker container makes the setup and build of mr4c a lot nicer.
But, the image has to match the hadoop OS cluster you are running because your libs, will be "version" matched and compiled.
And these versions in the coker image must match what is on the cluster.
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

