
# Solution for errors

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
5. run composer install
```
 "extra": {
        "patches": {
            "drupal/core": {
                "layoutbuilder-field-limit": "https://www.drupal.org/files/issues/2020-10-23/3029830-26-layout_builder_field_limit.patch"
            }
        },
```

### whenever you see Assert.php error
```zsh
dc up -d --f
```

error browserslist@4.14.0: The engine "node" is incompatible with this module. Expected version "^6 || ^7 || ^8 || ^9 || ^10 || ^11 || ^12 || >=13.7". Got "13.1.0"
error Found incompatible module.


yarn install --ignore-engines
./node_modules/.bin/gulp


### Rebuild specific container
`
 dc up -d --b cli
`


### When you can't do sql-sync
1. could not load API JWT Token, error was 'lagoon@ssh.laggon.amazeeio.cloud: Perission denide (publickey).

```
dc up -d --f cli
```
## Access to @stage
```
drush @stage ssh
drush uli
```


## empty entity errir
1. sequel pro > send query
```
update paragraphs_item_field_data
set parent_field_name="field_paragraphs", parent_id=30, parent_type="node"
where parent_id=""
```
2. send another one
```
update paragraphs_item_revision_field_data
set parent_field_name="field_paragraphs", parent_id=30, parent_type="node"
where parent_id="" 
```

3. flush cache


## PHP Fatal error:  Allowed memory size of **bytes exhausted
```
export COMPOSER_MEMORY_LIMIT=-1

//run this in container, and try again
```


## Lagoon server error 500
1. run the logs
```
docker-compose logs -f nginx
```
2. refresh the page - still not working then,
3. Remove vendor folder, composer install again 
4. try composer dump-autoload -o / composer dumpautoload


## sql-sync error - permission denied
```
[matui]cli-drupal:/app$ drush sa
Could not load API JWT Token, error was: 'lagoon@ssh.lagoon.amazeeio.cloud: Permission denied (publickey).                                                                                               [warning]
Could not load API JWT Token, error was: 'lagoon@ssh.lagoon.amazeeio.cloud: Permission denied (publickey).                                                                                               [warning]
@none
@self
default
```
check the pygmy status,
then up the cli container --force
```
dcupd --f cli
```


## pre-commit.config
1. Fix phpcs linting error
```
//1. run outside of container
vendor/bin/phpcs -sp --standard=Drupal --colors --extensions=php,inc,module,install web/modules/custom/thm_modules/


// change s to bf like below to use auto fix
vendor/bin/phpcbf -sp --standard=Drupal --colors --extensions=php,inc,module,install web/modules/custom/thm_modules/
```


## Docker error approach
```
// check status
docker stats

// chek what happend when you call the sites
curl -IL http://property-inspection-plus.docker.amazee.io/  
```

## Docker php error approach


dcps

// grab the php name
docker logs $phpname


## Sql-sync Error 

1. try to sql-drop (remove db) first then sql-connect the DB you downloaded. it will be in the temp directory

2. check the temp directory, find the db file
3. and connect that db ('<' defines which direction DB will flows)
```
//from root
ll /tep/ 

drush sql-connect < $db.sql
```
4. if it's still failed, run the debug flag (if you add many vvv, more detail you could see), and find the what is the issue
```
drush sql-sync @source @destination -y -d -vvv
``` 
5.  
```
drush sql-query --help
```


## pymy or docker - Ports are not available it's already used

1. check what it is running on that port number.
```
lsof -i :$portNo
```

2. Kill it that port number
```
kill -9 $PID-number
```

3. check one more time if it's killed properly


#### port number - mailhog case
even if you killed it, still it's running on different PID number then, 
```
// find something running name has string 'mail'
ps aux | grep mail 
```
2. if it's in you local/opt/ then check the brew
```
brew list

brew info mailhog

brew servecies stop mailhog
```


### Lagoon build gyp python error
update the dockerfile

```
RUN apk add --no-cache \
  g++ \
  make \
  libtool \
  automake \
  autoconf \
  python2 \

```

### when you see the error in drupal "encountered an unexpected error"
```
// enable dblog
drush en dblog

// run
drush ws
```

## Switch composer version
```
//version 1
composer self-update --1
```


### when you have composer install issue becuase of the applied patch.

1. identify which patch causing issue from error messages
2. go to composer.json, and find the patch
3. google drupal $module_name > click the one of issue > replace the id from the url with the patch's one
4. check if its already commited. if it is, click and check which version it is. 
5. if installed version is too low, update our module, otherwise just remove the patch from the json file



# When 'http' does not work, only https works

### 1. Approach the issue
- Make sure that Mac os Apache is not running. For example updating Mac os might re-enable apache
```
sudo lsof -i :80
```
- If you see processes running where the user is one www or _www or http then that means apache is running
```
ps aux | grep httpd
```

If you see more than one process then that means apache is running.  

### 2. Solution

If apache is running then stop it
```
sudo apachectl stop
```

You’s also make to make sure apache doesn’t start on boot
```
sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```

Re start pypgmy and you should be good to go.



# when you lost db
## how to create dump file in your local
```
#drupal7
drush sql-dump --result-file=db.sql

$drupal8, 9
drush sql:dump --result-file=../$filename.sql
```

## restore db (apply local dump file)
```
drush sql:query --file=$filename
```

### Bootstrap hook error inside of container
- it means you don't have database