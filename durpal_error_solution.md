
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