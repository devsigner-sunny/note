# How to use custom hook

1. Create the custom module
2. Find the right hook


## Form Alter - to create the additional button

```php

// Turn on the phpstorm preference -> search drupal -> format -> enable drupal and set the version.
//then when you type function blah, blah it will gives you options.


/**
 * Implements hook_form_alter().
 */
function cloudyards_form_alter(&$form, \Drupal\Core\Form\FormStateInterface $form_state, $form_id)
{
    //get the form id (string)
  print_r($form_id);
}

/**
 * Implements hook_form_FORM_ID_alter().
 */

//change the FORM_ID to the one I got from the above code
function cloudyards_form_FORM_ID_alter(&$form, \Drupal\Core\Form\FormStateInterface $form_state, $form_id)
{

}

```


## attach the buttons with wrapper to form action
```php

function cloudyards_form_user_login_form_alter(&$form, &$form_state, $form_id) {
  $form['cloudyards_password_reset'] = [
    '#type' => 'link',
    '#title' => t('Forgot password?'),
    '#weight' => 1,
    '#url' => Url::fromRoute('user.pass'),
  ];

  $form['actions']['cloudyards_button_wrapper'] = [
    '#type' => 'container',
    '#attributes'=> ['class' => ['btn-wrapper']],
  ];

  $wrapper = &$form['actions']['cloudyards_button_wrapper'];

  $wrapper['cloudyards_join_as_buyer'] = [
    '#type' => 'link',
    '#title' => t('Join as Buyer/Seller'),
    '#weight' => 2,
    '#url' => Url::fromRoute('user.register'),
    '#attributes' => ['class' => ['btn', 'btn-primary']],
  ];

  $wrapper['cloudyards_join_as_stock_request'] = [
    '#type' => 'link',
    '#title' => t('Join as Stock agent'),
    '#weight' => 3,
    '#url' => Url::fromRoute('entity.webform.canonical', ['webform' => 'join_as_stock_agent_request']),
    '#attributes' => ['class' => ['btn', 'btn-primary']],
  ];
}
```


## ModalkLInkController
- To open the link what we wanna show as Modal
- in Editor (such as gutenberg, ckeditor), when user create a link and that link meet some conditions following the code, user can open other node/taxonomy as Modal popup in Ajax.
```
// For example

<a class="use-ajax" data-dialog-type="modal" href="/modal_link/node/292?title=foo&amp;view_mode=teaser">Sssss</a>
```