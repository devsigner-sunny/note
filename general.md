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

## Debug
1. debug on - docker-compose.yml (uncommented XDEBUG_ENABLE)
2. reup php: dcupd --f php
3. Start phpstorm debug icon on
4. Chrome debug on
5. Make own stop point - Type dummy variable above the line you want to make stop point
```php
function youthtown_core_views_pre_render(ViewExecutable $view) {
  $yay = '';
  # let's say we wanna know $view->id 
  if ($view->id() == 'testimonial_slick_view') {
    $view->element['#attached']['library'][] = 'slick/slick';
  }
}
```
6. 2ways to figure it out what's the value of $view->id()
  - highlighted, right click -> evaluate
  - highlighted, right click -> add watchers




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