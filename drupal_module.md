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

  Drupal.behaviors.orchidSlick = {
    attach: function (context, settings) {
      const slickTargets = [
        {
          parent: '.slick',
          target: '.slick__slider',
          infinite: false,
        },
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