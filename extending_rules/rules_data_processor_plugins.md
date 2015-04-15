# Rules Data Processor Plugins

Data processors are for processing the values resulting from the configured data selection - Rules implements a processor for applying offsets to date values as well as a custom-PHP-code processor.

To implement a Rules Data Processor Plugin, place your plugin code in ```\Drupal\{module_name}\Plugin\DataProcessor\```, for example:

```
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
