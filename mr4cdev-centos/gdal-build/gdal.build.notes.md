history
```
  mkdir  /root/rpmbuild/SPECS/../SOURCES
  cd /root/rpmbuild/SOURCES
  wget http://download.osgeo.org/gdal/1.10.0/gdal-1.10.0.tar.gz
  yum -y install giflib-devel libjpeg-devel libpng-devel python-devel
  rpmbuild -ba SPECS/gdal.spec

```
