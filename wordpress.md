# Start new project
1. Copy from the old project (such as magnetic web or latest ver)
2. Delete .git file -> otherwise, git will recognise this as old project
2. Delete /vendor, to get latest plugin
3. Delete files inside of /Uploads
3. Go to web/app/plugin, compare the list with composer.json.
4. If we have the item in composer.json, delete the directory to get the latest ver.
5. Composer install.


# Access ssh
1. Get the project in local
2. access info in wp-cli.yml
3. no need to run the command inside of container.
```yml
@master:
   ssh: property-inspection-plus-master@ssh.lagoon.amazeeio.cloud:32222

```
```
ssh property-inspection-plus-master@ssh.lagoon.amazeeio.cloud -p 32222
```

## Install plugin local in Docker
```zsh
composer require wpackagist-plugin/plugin-name
wp plugin activate plugin-name
```