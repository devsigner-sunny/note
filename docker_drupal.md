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

#import dump file onto destination database`
drush sql-sync @master @self

#resync - get image files
drush rsync @master:%files @self:%files -y


#grab the file from the stage
drush rsync @stage:%files @self:%files


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


## Direct login throuh ssh
```
drush uli
```


## Email

### Testing email 
1. manually check
```php
<?php

/**
 * @file
 * This is the primary module file.
 */

use Drupal\Core\Routing\RouteMatchInterface;
use Drupal\Core\Render\Markup;
use Drupal\Core\Url;

include_once __DIR__ . '/includes/helpers/utilities.inc';

// Define permissions.
define('SWIFTMAILER_ADMINISTER', 'swiftmailer_administer');

// Define message formats.
define('SWIFTMAILER_FORMAT_PLAIN', 'text/plain');
define('SWIFTMAILER_FORMAT_HTML', 'text/html');

// Define transport types.
define('SWIFTMAILER_TRANSPORT_SMTP', 'smtp');
define('SWIFTMAILER_TRANSPORT_SENDMAIL', 'sendmail');
define('SWIFTMAILER_TRANSPORT_NATIVE', 'native');
define('SWIFTMAILER_TRANSPORT_SPOOL', 'spool');
define('SWIFTMAILER_TRANSPORT_NULL', 'null');

// Define header types.
define('SWIFTMAILER_HEADER_TEXT', 'text');
define('SWIFTMAILER_HEADER_PARAMETERIZED', 'parameterized');
define('SWIFTMAILER_HEADER_MAILBOX', 'mailbox');
define('SWIFTMAILER_HEADER_DATE', 'date');
define('SWIFTMAILER_HEADER_ID', 'ID');
define('SWIFTMAILER_HEADER_PATH', 'path');

// Define system variables defaults.
define('SWIFTMAILER_VARIABLE_RESPECT_FORMAT_DEFAULT', FALSE);
define('SWIFTMAILER_VARIABLE_CONVERT_MODE_DEFAULT', FALSE);
define('SWIFTMAILER_VARIABLE_PATH_DEFAULT', '');
define('SWIFTMAILER_VARIABLE_FORMAT_DEFAULT', 'text/plain');
define('SWIFTMAILER_VARIABLE_CHARACTER_SET_DEFAULT', 'UTF-8');

/**
 * Implements hook_mail().
 */
function swiftmailer_mail($key, &$message) {
  $user = \Drupal::currentUser();
  $message['headers']['Content-Type'] = SWIFTMAILER_FORMAT_HTML;


/* EDIT HERE, then TRY SWIFTMAILER TEST IN SYSTEM */

//  $text[] = '<h3>' . t('Dear @user,', ['@user' => $user->getDisplayName()]) . '</h3>';
//  $text[] = '<p>' . t('This e-mail has been sent from @site by the Swift Mailer module. The module has been successfully configured.', ['@site' => \Drupal::config('system.site')->get('name')]) . '</p>';
//  $text[] = t('Kind regards') . '<br /><br />';
//  $text[] = t('The Swift Mailer module');


/* manually put here*/
  $text[] = <<<TWIG
<table style="font-family:'Arial', sans-serif; font-size: 14px; line-height: 24px; ">
    <tbody>
        <tr>
            <td>
                <p style="margin-bottom: 20px;">Dear {{ name }},</p>
                <p>Thanks very much for subscribing to the Everybody Patient Sheets.</p>
                <p>When you have completed the sign-up process, and once you are logged in, you can access the Everybody Patient Sheets straight away by clicking on the Patient Sheets link on the top navigation bar on the {{ site_name }} website.</p>
                <p>To download the Everybody Patient Sheets click on the link below</p>
                <a href="{{ login_url }}" style="color:#fff; background:#bd1822; border: 10px solid #bd1822; border-left:30px solid #bd1822; border-right: 30px solid #bd1822; text-decoration:none; display: inline-block; text-align: center; margin: 1em auto;">Access Everybody Patient Sheets</a>
            </td>
        </tr>
        <tr>
            <td>
                <p>We would love you to check your order and let us know if we need to make any changes.</p>

                <h3>Your details in our system are:</h3>
                <p style="margin-bottom: 20px;">Your email address/login name is: {{ email }}</p>
                <p style="margin-top: 0;">{{ full_name }}<br>
                {% for address_line in shipping_address %}
                    {% if address_line %} {{ address_line }}<br> {% endif %}
                {% endfor %}
                </p>
            </td>
        </tr>
    </tbody>
</table>
TWIG;

  $message['subject'] = t('Swift Mailer has been successfully configured!');
  $message['body'] = array_map(function ($text) {
    return Markup::create($text);
  }, $text);
}

/**
 * Implements hook_theme().
 */
function swiftmailer_theme($existing, $type, $theme, $path) {
  return [
    'swiftmailer' => [
      'variables' => [
        'message' => [],
      ],
      'mail theme' => TRUE,
    ],
  ];
}

/**
 * Implements hook_theme_suggestions_HOOK() for swiftmailer.
 */
function swiftmailer_theme_suggestions_swiftmailer(array $variables) {
  $suggestions = [];
  $suggestions[] = 'swiftmailer';
  $suggestions[] = 'swiftmailer__' . $variables['message']['module'];
  $suggestions[] = 'swiftmailer__' . $variables['message']['module'] . '__' . $variables['message']['key'];
  return $suggestions;
}

/**
 * Prepares variables for swiftmailer templates.
 *
 * Default template: swiftmailer.html.twig.
 *
 * @param array $variables
 *   An associative array containing:
 *   - message: An associative array containing the message array.
 *   - body: The processed body.
 *   - subject: The processed subject.
 *   - base_url: The base url for this site including scheme, without trailing slash.
 */
function template_preprocess_swiftmailer(&$variables) {
  $variables['base_url'] = $GLOBALS['base_url'];
  $variables['subject'] = $variables['message']['subject'];
  $variables['body'] = $variables['message']['body'];
}

/**
 * Implements hook_help().
 */
function swiftmailer_help($route_name, RouteMatchInterface $route_match) {
  switch ($route_name) {
    case 'help.page.swiftmailer':
    case 'swiftmailer.transport_settings':
    case 'swiftmailer.message_settings':
    case 'swiftmailer.test':
      $output = '';
      $output .= '<p>' . t('The Swift Mailer module is designed to replace the default mail system that is shipped with Drupal. The initial configuration of this is done through the <a href=":mailsystem_settings">Mail System module</a>. Swift Mailer allows you to choose how e-mails should be sent. To read more about how this module works, please have a look at the <a href=":documentation">Swift Mailer documentation</a>.', [':mailsystem_settings' => Url::fromRoute('mailsystem.settings')->toString(), ':documentation' => 'http://swiftmailer.org/docs/introduction.html']) . '</p>';
      return $output;
  }
}

```


2. all.settings.php
```
$config['user.mail']['password_reset']['body'] = <<<HTML
THIS IS A TEST.
<p>[user:one-time-login-url]</p>
HTML;
```



### Get into the lagoon
```
//make sure the container is running
drush @master ssh
```



## Pulling data (to sync between lagoon environments)
```
#Run a sql-sync
#SSH into the lagoon server, destination branch

# list all the files in the /tmp directory, & identify which one is the dump from source.
# it'll be named for the source branch and date/time.
ls /tmp

#unzip the sql file.
gunzip /tmp/project_master_123_20120-01-05-154535.sql.gz

# import the SQL file into the database
drush sql-query "source /tmp/project_master_123_20120-01-05-154535.sql"


#to get the files (images etc..)
drush rsync @master:%files @self:%files -y
```


## package has known vulnerabilities.

### if drupal/core
- remove web/core, vendoer/pear, composer.lock
- update compsoer.json to 8.9.15 -> composer update drupal/core-recommended