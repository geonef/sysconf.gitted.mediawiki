## A Sysconf profile

This is a [SYSCONF](https://github.com/geonef/sysconf.base)
profile. SYSCONF is a method and tool to manage custom system files
for easy install, backup and sync.

This profile provides a [MediaWiki](http://mediawiki.org/) service.
MediaWiki is a free software open source wiki package written in PHP,
originally for use on Wikipedia. It is now also used by several other
projects of the non-profit Wikimedia Foundation and by many other
wikis, including this website, the home of MediaWiki.

* See Howto: [Import live MediaWiki](doc/import-live-mediawiki.md)

## Services

```
# netstat -tlpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN      8065/php-fpm.conf)
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -               
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      8044/nginx      
```

* The main service is NginX running on port 80.
* You can also access the MySQL service on the port 3306.


## Gitted import/export

This profile will import/export:

* MySQL data in the ```mysql/``` directory from/to the MySQL database
  ```mediawiki``` (see
  [etc/gitted/sync/](sysconf/tree/etc/gitted/sync/) for the
  responsible scripts)

* SYSCONF files in the ```sysconf/``` directory from/to container
  living ```/sysconf```. This is
  [provided](https://github.com/geonef/sysconf.gitted/tree/master/tree/etc/gitted/sync)
  by the
  [sysconf.gitted profile](https://github.com/geonef/sysconf.gitted).



## Gitted integration

* To create a new Gitted repository, follow the instructions at
  [How to setup Gitted for an application](https://github.com/geonef/sysconf.gitted/blob/master/doc/howto-create-new.md)
  
* Then add this Sysconf profile:
```
git subtree add -P sysconf/sysconf.gitted.mediawiki git@github.com:geonef/sysconf.gitted.mediawiki.git master
```

* Integrate it in the dependency chain, for example:
```
echo sysconf.gitted.mediawiki >sysconf/actual/deps
```

* Then push it to the container:
```
sysconf/gitted-client register
sysconf/gitted-client add mw
git push mw master
```


## Authors

Written by Jean-Francois Gigand <jf@geonef.fr>. Feel free to contact me!
