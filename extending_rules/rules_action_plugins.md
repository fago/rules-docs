# Rules Action Plugins

To implement a Rules Action Plugin, place your plugin code under the namespace ```\Drupal\{module_name}\Plugin\RulesAction\``` in ```{module_name}/src/Plugin/RulesAction```, for example:

```php
/**
 * Provides a 'Delete entity' action.
 *
 * @RulesAction(
 *   id = "rules_entity_delete",
 *   label = @Translation("Delete entity"),
 *   category = @Translation("Entity"),
 *   context = {
 *     "entity" = @ContextDefinition("entity",
 *       label = @Translation("Entity"),
 *       description = @Translation("Specifies the entity, which should be deleted permanently.")
 *     )
 *   }
 * )
 */
class EntityDelete extends RulesActionBase {

  /**
   * {@inheritdoc}
   */
  public function execute() {
    $entity = $this->getContextValue('entity');
    $entity->delete();
  }

}
```
This replaces Drupal 7 Rules' ```hook_rules_action_info```
