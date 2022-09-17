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

# Create account
1. access to ssh
2. ls -> go to web/
3. run command, you will get the email
```
wp user update heather@communica.co.nz --user_pass=put-password-here
```

## Install plugin local in Docker
```zsh
composer require wpackagist-plugin/plugin-name
wp plugin activate plugin-name

composer remove wpackagist-plugin/plugin-name
```

## Wordpress - Grabaloan go live
1. Backup live database
2. Enable maintenance mode
3. Git merge stage to live
4. Copy database over to live
```
ssh grabaloan-master@lagoon 
```

- To do that, you may need to change config file first
```
Host lagoon
  Hostname ssh.lagoon.amazeeio.cloud
  Port 32222
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
```

5. Copy files over
6. Search and replace url
7. Make site live


# Start project with backup file (existing site)

### Set up local dev only
1. grab the docker.wp.exmaple file  -> change the name to your project name
2. copy the directory  "homedir > public_html" from backup file and paste to our project file, rename as "web"
3. update wp-config.php file
```
DB_USER: lagoon
DB_PASSWORD: lagoon
MySQL: mariadb
```
4. composer install
5. import data from backupfile > mysql > data.sql 
6. or copy the data.sql file and paste in project then use the command line
```
//inside of container, to access mariadb
mysql -u lagoon -plagoon -h mariadb lagoon

//import
source /app/indianink_production.sql 

//check 
show tables;
```
7. change the siteurl value to prevent redirect to live site

```
//inside of maria db, check the existing value
select * from ii_options where option_name in ('siteurl', 'home')

//update value
 update ii_options set option_value = 'http://indian-ink.docker.amazee.io' where option_name in ('home', 'siteurl');


// to see other options rather than just siteurl
select option_name from ii_options order by option_name;
```

8. if still it links to live site, check the incognito mode, if it's working right it means your brower cache setting issue
   - you need to clear brower dns cache
   - navigate to "chrome://net-internals/#dns"

9. If you can't use wp-admin, check if they use some plugin to hide path
   - show all options 
   - check the suspicious table, try to get

```
select option_name from ii_options order by option_name;

// could be
select * from ii_options where option_name = 'hidecode';
select * from ii_options where option_name like 'whl_%';

//update
update ii_options set option_value = 'wp-admin' where option_name = 'whl_redirect_admin';

```


## get user data to login
```
select * from ii_users;
```



# import db manually
1. get the db from lagoon dashboard (download backup file)
2. change the name data.sql and move into project directory
3. inside of container run this command
```
wp db import $filename.sql
```
