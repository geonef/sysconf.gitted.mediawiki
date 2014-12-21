## A Sysconf profile

This is a [SYSCONF](https://github.com/geonef/sysconf.base)
profile. SYSCONF is a method and tool to manage custom system files
for easy install, backup and sync.

This profile provides a [Tiny Tiny RSS](http://tt-rss.org/) service. tt-rss is a power,
free-software news reader running with PHP.

## Services

```
# netstat -tlpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN      21143/php-fpm.conf)
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      7799/nginx      
tcp        0      0 0.0.0.0:5432            0.0.0.0:*               LISTEN      29057/postgres  
tcp6       0      0 :::5432                 :::*                    LISTEN      29057/postgres  
```

* The main service is TinyTinyRSS running on port 80.
* You can also access the PostgreSQL service on the port 5432.


## Gitted import/export

This profile does not import/export anything by itself.

But its dependencies:
* sysconf.gitted [provides import/export](https://github.com/geonef/sysconf.gitted/tree/master/tree/etc/gitted/sync) of the ```sysconf/``` directory

* sysconf.gitted.postgresql
  [provides import/export](https://github.com/geonef/sysconf.gitted.postgresql/tree/master/tree/etc/gitted/sync)
  of PostgreSQL data in the ```postgresql/``` directory.


## Gitted integration

* To create a new Gitted repository, follow the instructions at
  [How to setup Gitted for an application](https://github.com/geonef/sysconf.gitted/blob/master/doc/howto-create-new.md)
  
* Then add this Sysconf profile:
```
git subtree add -P sysconf/sysconf.gitted.tt-rss git@github.com:geonef/sysconf.gitted.tt-rss.git master
```

* Integrate it in the dependency chain, for example:
```
echo sysconf.gitted.tt-rss >sysconf/actual/deps
```

* Then push it to the container:
```
sysconf/gitted-client register
git push <name> master
```


## Authors

Written by Jean-Francois Gigand <jf@geonef.fr>. Feel free to contact me!
