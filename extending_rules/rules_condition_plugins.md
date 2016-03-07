# Rules Condition Plugins

To implement a Rules Condition Plugin, place your plugin code under the namespace ```\Drupal\{module_name}\Plugin\Condition\``` in ```{module_name}/src/Plugin/Condition```, for example:

```php
/**
 * Provides a 'Node is sticky' condition.
 *
 * @Condition(
 *   id = "rules_node_is_sticky",
 *   label = @Translation("Node is sticky"),
 *   category = @Translation("Node"),
 *   context = {
 *     "node" = @ContextDefinition("entity:node",
 *       label = @Translation("Node")
 *     )
 *   }
 * )
 */
class NodeIsSticky extends RulesConditionBase {

  /**
   * Check if the given node is sticky.
   *
   * @param \Drupal\node\NodeInterface $node
   *   The node to check.
   *
   * @return bool
   *   TRUE if the node is sticky.
   */
  protected function doEvaluate(NodeInterface $node) {
    return $node->isSticky();
  }

}
```
This replaces Drupal 7 Rules' ```hook_rules_condition_info```
