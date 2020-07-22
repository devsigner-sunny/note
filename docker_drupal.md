# Setting up

1. docker desktop should be running,

1. Git clone

1. Get the data from docker

```zsh
pygmy up

pygmy status

composer install

#check .gitmodules if it has submodule
#then, pulling all the submodules

git submodule update --init --recursive

docker-compose up -d

docker-compose ps

#get into the drush (alias: dcd)
docker-compose exec cli bash

#see a list of available site aliases
drush sa

#import dump file onto destination database
drush sql-sync @master @self

#resync
drush rsync @master:%files @self:%files -v


#if you can't download it using above line, USE sequel pro.
#get port number from docker

```

4. Make your own settings.local.php inside of site > default

- get the configuration from someone

5. Connect host to detect each other.

- change the Gateway number (extra_hosts) on docker-compose.yml

6. How to know my gateway number

```zsh
docker network ls

#then, copy the right name you are working on

docker inspect $name
```

## Drupal module/config

```zsh
#download
composer require drupal/$modulename

#import: config import (optional -y)
dcd cim -y

#enable
dcd en $modulename -y

#export (to share)
dcd cex

#then, make sure to commit the change of configuration
```

---

## Solution for errors

1. clear cache drupal8, drupal7

```zsh
dcd cr
dcd cc all
```

2. git pull, then check if you have any .yml file or modules.

```zsh
#moudles
composer install

#.yml
dcd cim -y
```

3. Rebuild

```zsh
dcd up -d --force
```

### Reset local user db

```zsh
dcd sqlsan --sanitize-password=KG5n9yJrGZR --sanitize-email=no --whitelist-fields=field_lms_client_id,field_real_email
```

### When you meet server error 502 or 500

1. run below code
```zsh
docker-compose logs -f nginx
```
2. copy the error code, and google it
3. Find patch (usually green highligted, check version)
4. Click it and copy the url into composer.json "patches"