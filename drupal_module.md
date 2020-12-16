# module


## Drupal module/config

```zsh
#download
composer require drupal/$modulename --no-update
composer update durpal/$modulename

#import: config import (optional -y)
dcd cim -y

#enable
dcd en $modulename -y

#export (to share)
dcd cex

#then, make sure to commit the change of configuration
```

## When your module not working properly
1. Check If it needs any libraries as requirements on document.
2. Check the library is generated and loaded (web/libraries)
3. Check the status report nothiing complain it
4. Check console error


## Entity Queue
    - nstall entity queue
    - Create entity queue
    - Jump to view block -> Advanced: Add relationships
    - Add sort criteria: Entityqueue: Content Queue Position
    - and delete original setting sort criteria
    - Add filter criteria: Entytyqueue: Content in Queue (=Yes)
    - Then, update preview


## Slick - (ref. youthtown)
**Not only slick, if the module requires their own library, Do the same step**

### Install
1. composer.json: install slick, slick view, kenwheeler/slick --- read requirement on module page (blazy)
2. package.json: check the dependencies - blazy
3. lagoon/install.sh
```sh

#if it doesnt have libraries directory, make it
if [ ! -d $MY_PATH/web/libraries ]; then mkdir -p $MY_PATH/web/libraries; fi

# Link library files
cd $MY_PATH/web/libraries && ln -sf ../../vendor/kenwheeler/slick/

# Move library files
if [ ! -d  $MY_PATH/web/libraries/blazy ]
then
    cp -r $MY_PATH/node_modules/blazy $MY_PATH/web/libraries/blazy
fi

```
4. If it still doesnt work, check slick.js file loaded 
5. If not loaded, debug at projectname_core.module - attach the file before render
```php
/**
 * Implements hook_views_pre_render(). 
 */
function youthtown_core_views_pre_render(ViewExecutable $view) {
  if ($view->id() == 'testimonial_slick_view') {
    $view->element['#attached']['library'][] = 'slick/slick';
  }
}
```

6. If we use **slick on GUTENBERG**

- 1) Gutneberg renders content through javascript, we need to initialize slick after everything is loaded
```javascript
// modules > custome > $projectname_core > js > create that file: youthtown_slick.js

/**
 * @file
 * Orchid jssor slider instantiation and related functions.
 */

(function ($, Drupal) {
  'use strict';

  Drupal.behaviors.youthtownSlick = {
    attach: function (context, settings) {
      const slickTargets = [
        {
          parent: '.slick',
          target: '.slick__slider',
          infinite: false,
        },
        {
          parent: '.view-gallery',
          //parent level of slide
          target: '.field-content',
          settings: {
            slidesToShow: 3,
            dots: true,
            infinite: false,
          }
        }
      ];

      $(document).ready(function () {
        slickTargets.forEach(function (slickTarget) {
          if ($(slickTarget.parent.length !== 0)) {
            const target = slickTarget.parent + ' ' + slickTarget.target;
            $(target).slick(slickTarget.settings);
          }
        });
      });
    }
  };
})(jQuery, Drupal);




```

- 2) For every page we add those library
```php
# modules > custom > $projectname_core > $projectname_core.module

/**
 * Implements hook_page_attachments_alter().
 */
function youthtown_core_page_attachments_alter(array &$attachments) {
  $attachments['#attached']['library'][] = 'slick/slick';
  $attachments['#attached']['library'][] = 'youthtown_core/slick';
}
```

### Setup
1. enable slick, slick view, slick ui, balzy
2. config > media > slick (setup the slick)
3. view block > format: Slick carousel, settings > Optionset Main: select I created on step2
 


 ## Colorbox
 1. Install the libraries through the documents by downloading (place the directory colorbox inside of libraries directory)
 2. config > medida > slick (setup the slick)
 3. view block > fields: $item > formatter: blazy, image style, media switcher: image to colorbox
 4. If it's not working, then install the add the code our own js (slick step6. module/custom/$project_name_core/js)
 5. If still not working, add below code to lagoon/install.sh, and package.json
 ```zsh

# install.sh
 # Move library files
if [ ! -d  $MY_PATH/web/libraries/blazy ]
then
    cp -r $MY_PATH/node_modules/blazy $MY_PATH/web/libraries/blazy
fi

if [ ! -d  $MY_PATH/web/libraries/colorbox ]
then
    cp -r $MY_PATH/node_modules/jquery-colorbox $MY_PATH/web/libraries/colorbox
fi
 ```
 ```json
  "dependencies": {
    "blazy": "^1.8.2",
    "jquery-colorbox": "^1.6.4"
  }
 ```