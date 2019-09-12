## Drupal common entity route 

### Node Route

| Links Key | Route Name | Route Example URI |
| --- | --- | --- |
| canonical | entity.node.canonical | /node/1 |
| add-page | entity.node.add_page | /node/add |
| add-form | entity.node.add_form | /node/add/article |
| delete-form | entity.node.delete_form | /node/1/delete |
| collection | entity.node.collection | /admin/content |


## Drupal create link with target blank
```php
// Use route
$options = ['absolute' => TRUE, 'attributes' => ['target' => '_blank']];
$link_object = Drupal\Core\Link::createFromRoute(t('the general terms and conditions of business'),
    'entity.node.canonical', ['node' => "123"],
    $options);
$link = $link_object->toString();

// Use url
$options = ['absolute' => TRUE, 'attributes' => ['target' => '_blank']];
$link_object = Link::fromTextAndUrl('Detail', Url::fromUserInput('/node', $options));
$link = $link_object->toString();

```

## Drupal logger object as array
```php
   \Drupal::logger('module_name')->notice('<pre><code>' . print_r($array_to_print, TRUE) . '</code></pre>'    );
```

## Drupal form ajax command
```
https://www.drupal.org/docs/8/api/javascript-api/ajax-forms
https://www.drupal.org/docs/8/api/ajax-api/core-ajax-callback-commands
https://www.drupalexp.com/blog/creating-ajax-callback-commands-drupal-8
```

## Override service class
```
https://kevinquillen.com/overriding-services-drupal-8-serviceprovidebase
https://www.bounteous.com/insights/2017/04/19/drupal-how-override-core-drupal-8-service/
```

## Theme suggestions
```
https://sqndr.github.io/d8-theming-guide/theme-hooks-and-theme-hook-suggestions/theme-hook-suggestions.html
```

## Theme preprocess
```
https://developpeur-drupal.com/en/article/theme-preprocess-and-suggestions-drupal-8-bootstrap-sub-theme
```

## Twig function for Drupal 8
```php
https://www.drupal.org/docs/8/theming/twig/functions-in-twig-templates
https://www.drupal.org/docs/8/modules/twig-tweak/cheat-sheet
```

## Check request is admin route
```php
if (\Drupal::service('router.admin_context')->isAdminRoute()) {
 // do stuff
}

```
Reference: https://drupal.stackexchange.com/a/219371/1542


## List Drupal 8 form api elements
```php
https://api.drupal.org/api/drupal/elements/8.7.x
https://drupalize.me/tutorial/form-element-reference?p=2766
```

## Add ajax for reset button in exposed filter views
```php
/**
 * Implements hook_form_BASE_FORM_ID_alter().
 */
function MODULE_form_views_exposed_form_alter(&$form, FormStateInterface $form_state, $form_id) {
  $storge = $form_state->getStorage();
  if (!empty($storge['view']) && $storge['view']->id() === 'my_view') {
    if (isset($form['actions']['reset']) && isset($form['actions']['submit'])) {
      $submit_id = $form['actions']['submit']['#id'];
      $form['actions']['reset']['#attributes']['onclick'] = 'javascript:jQuery(this.form).clearForm();jQuery("#' . $submit_id . '").trigger("click");return false;';
    }
  }
}

```
## Use Drupal Ajax Command with javascript 
```js
var ajax = new Drupal.Ajax(false, false, {
  url: Drupal.url('path/to/controller')
});
ajax.execute().done(function () {
  alert('Done');
});
```

## Letencrypt
```php
https://gist.github.com/flashvnn/82f8707b141b6adfee998d9588fac927
```

## Drupal 8 install offline with other language
```php
https://drupal.stackexchange.com/questions/164172/problem-installing-in-local-the-translation-server-is-offline
Copy translate file to folder sites/default/files/translations
```

## File with Drupal 8

```php
// Move file entity.
$file = \Drupal\file\Entity\File::load($fid);
file_move(FileInterface $source, 'public://newlocation/demo.jpg');

```

## Edit Drupal config
```php
  $config_factory = \Drupal::configFactory();
  $langcode = $config_factory->get('system.site')->get('langcode');
  $config_factory->getEditable('system.site')->set('default_langcode', $langcode)->save();
```

## Check entity bundle has translated
```php
  if(!\Drupal::has('content_translation.manager')){
    return false;
  }
  $content_translation_manager = \Drupal::service('content_translation.manager');
  $compatible = $content_translation_manager->isEnabled('node', 'general');
```


## Get list field of bundle
```php
$entityManager = \Drupal::service('entity_field.manager');
$fields = $entityManager->getFieldDefinitions($entity_type, $bundle);
```

## Get entity bundle of entity type
```php
  /**
   * Get entity bundle options.
   *
   * @param $entity_type_id
   *   The entity type identifier.
   *
   * @return array
   *   An array of entity bundle options.
   */
  protected function getEntityBundleOptions($entity_type_id) {
    $options = [];

    foreach ($this->entityTypeBundleInfo->getBundleInfo($entity_type_id) as $name => $definition) {
      if (!isset($definition['label'])) {
        continue;
      }
      $options[$name] = $definition['label'];
    }

    return $options;
  }
```


## Set default value for view filter
```php
/**
 * Implements hook_form_BASE_FORM_ID_alter().
 */
function MODULE_form_views_exposed_form_alter(&$form, FormStateInterface $form_state, $form_id) {
  $storge = $form_state->getStorage();
  if ($storge['view']->id() === 'view_news') {
    if (isset($form['changed_date_select']['#options']) && count($form['changed_date_select']['#options']) > 1) {
      if (isset($form['changed_date_select']['#options']['All'])) {
        $user_input = $form_state->getUserInput();
        if (isset($user_input['changed_date_select']) && $user_input['changed_date_select'] === 'All') {
          next($form['changed_date_select']['#options']);
          $key = key($form['changed_date_select']['#options']);
          $form['changed_date_select']['#default_value'] = $key;
          $user_input['changed_date_select'] = $key;
          $form_state->setUserInput($user_input);
        }
      }
    }
  }

}
```

## Check request is AJAX Form Request
```php
$request->query->has(static::AJAX_FORM_REQUEST))
```

### Add basic authetication for /user url
```
# Add in virtualhost config
<Location /user>
  AuthUserFile /home/path/.htpasswd
  AuthName "Password Protected Area"
  AuthType Basic
  Require valid-user
</Location>

```

## Install Acquia Dev Desktop
Install Microsoft Visual C++ 2010 Redistributable Package before run Acquia Dev Desktop
```
https://www.microsoft.com/en-us/download/details.aspx?id=14632
https://www.microsoft.com/en-us/download/details.aspx?id=5555
```

## Issue: Drupal Console PHP Fatal error:  require(): Failed opening required 'drupal.php'
Check PHP installed with ionCube, need remove it 

## Issue: Drupal console: The “--shellexec_output” option does not exist
Remove the line with shellexec_output: true from ~/.console/config.yml (probably the last line in the file).

## Install composer 

```bash
curl -sS https://getcomposer.org/installer -o composer-setup.php
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

## Install drupal console global
```bash
curl https://drupalconsole.com/installer -L -o drupal.phar
mv drupal.phar /usr/local/bin/drupal
chmod +x /usr/local/bin/drupal
```

## Render array Properties
**#type, #theme, and #markup**
Almost every Render API element at any level of the render array's hierarchy will have one of these properties defined.

**#plain_text**

Specifies that the array provides text that needs to be escaped.
Example:
```
'#plain_text' => t('Hello world!')
```
