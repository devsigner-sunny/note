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



# Solutions

## To give permission
```
sudo chmod 744 $directory_name
```


## When you commit, memory issue
```
//try first 
export COMPOSER_MEMORY_LIMIT=-1

//if still need more, go to php --ini
php --ini 

//then, you will see the path of configuration, open the file -> search memory-limit then changethe number.

```


## Import google font 
- Do not import through the sass file, but add child_theme.libraries.yml

```
global-styling:
  css:
    theme:
      css/font-awesome.min.css: {}

      **** Need to have // ***
      //fonts.googleapis.com/css2?family=Roboto:wght@100&display=swap: {}

```