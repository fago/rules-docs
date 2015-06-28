# Rules Data Processor Plugins

Data processors are for processing the values resulting from the configured data selection.

To implement a Rules Data Processor Plugin, place your plugin code under the namespace ```\Drupal\{module_name}\Plugin\RulesDataProcessor\``` in ```{module_name}/src/Plugin/RulesDataProcessor```, for example:

```php
/**
 * A data processor for applying numerical offsets.
 *
 * The plugin configuration must contain the following entry:
 * - offset: the value that should be added.
 *
 * @RulesDataProcessor(
 *   id = "rules_numeric_offset",
 *   label = @Translation("Apply numeric offset")
 * )
 */
class NumericOffset extends PluginBase implements DataProcessorInterface {

  /**
   * {@inheritdoc}
   */
  public function process($value) {
    return $value + $this->configuration['offset'];
  }

}
```

This replaces Drupal 7 Rules' ```hook_rules_evaluator_info```
