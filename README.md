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
So you need to mount your code as a volume into the image (I use /opt/projects/) within the container.
Adjust as required.

# Ubuntu Latest Image (15)
```
cd mr4cdev-ubuntu
README.md
docker build -t mr4cdev-ubuntu .
docker run -ti --volume `pwd`/myproject-src:/opt/projects  mr4cdev-ubuntu /bin/bash
```


# Centos 6 Image
This is the tricky one, Centos 6 is a little old :-) 
```
cd mr4cdev-centos
README.md
docker build -t mr4cdev-centos .
docker run -ti --volume `pwd`/myproject-src:/opt/projects mr4cdev-centos /bin/bash
```

