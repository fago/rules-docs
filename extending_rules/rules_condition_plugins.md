# Rules Condition Plugins

To implement a Rules Condition Plugin, place your plugin code in ```\Drupal\{module_name}\Plugin\Condition\```, for example:

```
/**
 * Provides a 'Node is sticky' condition.
 *
 * @Condition(
 *   id = "rules_node_is_sticky",
 *   label = @Translation("Node is sticky"),
 *   category = @Translation("Node"),
 *   context = {...}
 * )
 */
class NodeIsSticky extends RulesConditionBase {

  /**
   * {@inheritdoc}
   */
  public function evaluate() {
    $node = $this->getContextValue('node');
    return $node->isSticky();
  }

}
```
This replaces Drupal 7 Rules' ```hook_rules_condition_info```
