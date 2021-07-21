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


## Create preprocesse hook
1. Find the right file to check the value you arelooking for on inspect mode (where that value coming from eg.twig)
2. Find the keyword to search it (eg.<title>{{ head_title|safe_join(' | ') }}</title>)
3. Search that keyworkd to see if we can edit/override
4. Grab that func name (function template_preprocess_html(&$variables) {) and google it to know how to use it
5. web > themes > custom > $themname > $themename.theme (where you write down your code)
6. Debuger on, restart php container(dcupd --f php), make a breakpoint by creating variable with empty value
```php
function youthtown_preprocess_html(&$variables) {
  //breakpoint 
 $boo = ''; 
}
```
7. If you add new func, you should flush cache all
8. Refresh the page, check it stops at breakpoint
9. Find the value you are looking for and right click "evaluate expression"


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

# Access Someone's local project 
```
sudo nano /etc/hosts
```

then write down the information given by person who share
```
192.168.1.26 jtwd.docker.amazee.io 
```
and  press enter


## permission
```
ls -al

//human readable
ls -alh 

//Recursive
chmod -R $directoryname
```
1. first 3 : root read write excute
2. second 3 : group
3. last 3: annonymouse


## check what happens when you access a website
```
curl -Il https://www.magneticweb.nz/wp-admin    
```


## Set up Host alias
```
ls ~/.ssh

vi ~/.ssh/config

// get inside
Host lagoon
  HostName ssh.lagoon.amazeeio.cloud
  Port 32222
  IdentityFile ~/.ssh/id_rsa

//If you have a warning : Remot host identification has changed
rm ~/.ssh/known_hosts  

then,
ssh zagga-nz-public-master@lagoon


## Show the where template comes from
copy the deployment.service.yml from other project ---- site/default/