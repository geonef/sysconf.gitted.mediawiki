# How to import MediaWiki content from a live wiki

## Method 1: use importDump and importImages

* See: http://www.mediawiki.org/wiki/Manual:Importing_XML_dumps

* Start with a fresh _gitted.mediawiki_ (without a ```mysql/``` directory)
* Log into the server of the existing live mediawiki install
* Use ```maintenance/dumpBackup.php`` to export all pages:
```
sudo -u www-data -g www-data php maintenance/dumpBackup.php --full --logs >/tmp/wiki.dump.xml
```
* Export all images
```
mkdir /tmp/wiki-images
cd /path/to/mediawiki/images
find 0 1 2 3 4 5 6 7 8 9 a b c d e f archive -type f -exec cp {} /tmp/wiki.images \;
cd /tmp
tar cvf /tmp/wiki.images.tar wiki.images
```
* Copy ```/tmp/wiki.dump.xml``` and ```/tmp/wiki.images.tar``` to the new,
  fresh gitted.mediawiki container's ```/tmp```, using ```scp``` or a
  pipe like: ```ssh user@live-host "cat /tmp/wiki.dump.xml" |
  lxc-attach -n mw -- su -c "cat >/tmp/wiki.dump.xml"``` (do the same
  for ```wiki.images.tar```)
* Run:
```
cd /tmp
tar xvf wiki.images.tar
cd /var/lib/mediawiki
sudo -u www-data -g www-data php maintenance/importDump.php /tmp/wiki.dump.xml
sudo -u www-data -g www-data php maintenance/importImages.php /tmp/wiki.images
sudo -u www-data -g www-data php maintenance/rebuildall.php
```

### Notes

* All images (and other files) are re-imported with the NOW date (but
  MW content of Media articles are preserved like articles in all
  Mediawiki namespaces)
  

## Method 2: copy database

* Take a dump of the MySQL database and restore it into the container.
* You can use Gitted:
  * put the dump into this Git repository as ```mysql/zzz.sql```
  * ```git add mysql/zzz.sql && git commit -m "imported dump"```
  * ```git push mw +master```
  * ```git pull mw master```

With this method, ```mysql/zzz.sql``` is blindly imported by the
Gitted scripts into the MySQL database. The ```git pull``` will
instead fetch proper ```mysql/``` files, that is, table data and
structures in separate .sql and .txt (CSV) files.


### Converting from PostgreSQL to MySQL

* This is
  [another problem](https://www.google.com/?q=converting%20postgresql%20to%20mysql),
  but it should work.
  
