# Notes

gdal 1.10 for el6 has been really hard to locate.
So .. here is the setup to build an RPM for it.

https://github.com/Kortforsyningen/rpmbuild/blob/master/SPECS/gdal.spec

untested

# From my build history
```
  mkdir  /root/rpmbuild/SPECS/../SOURCES
  cd /root/rpmbuild/SOURCES
  wget http://download.osgeo.org/gdal/1.10.0/gdal-1.10.0.tar.gz
  yum -y install giflib-devel libjpeg-devel libpng-devel python-devel
  rpmbuild -ba SPECS/gdal.spec

```


# Included in this Dockerfile ..
is the links to where gdal 1.11 was found (RPMs) for el6
