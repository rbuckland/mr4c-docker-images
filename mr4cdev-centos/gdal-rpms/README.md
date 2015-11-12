# gdal for EL6 (Centos 6)

These took a LONG time to locate.

```
cat gdal-package-list.txt | awk '{print $1}' | xargs -L1 -Ixx wget http://ftp.tmapy.cz/tmapy-twist/centos/6/testing/x86_64/RPMS/xx
```

You may want to inspect the src-rpm as well which is located in this same folder.


