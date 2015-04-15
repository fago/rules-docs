# Rules Action Plugins

To implement a Rules Action Plugin, place your plugin code in ```\Drupal\{module_name}\Plugin\Action\```, for example:

```
/**
 * Provides a 'Delete entity' action.
 *
 * @Action(
 *   id = "rules_entity_delete",
 *   label = @Translation("Delete entity"),
 *   category = @Translation("Entity"),
 *   context = {...}
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
