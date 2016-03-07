#Rules Plugin Derivatives Policy

The Plug-in System of Drupal 8 provides a mechanism called [Plugin Derivatives](https://www.drupal.org/node/1653226). This powerful concept allows a single plugin class to expose multiple plugins to the user interface.

In Rules, currently we use plugin derivates whenever dealing with entities: Instead of exposing a single "Create a new Entity" action, the user is presented with options like "Create a new Node", "Create a new Taxonomy Term" or "Create a new User".

According to our [Derivatives policy](https://www.drupal.org/node/2473169), we want to have derivatives whenever dealing with entity type based Actions or Conditions. Other cases might make sense, feel free to discuss them in the [policy issue](https://www.drupal.org/node/2473169).

A good reference implementation is the EntityCreate action:


The [EntityCreate](https://github.com/fago/rules/blob/8.x-3.x/src/Plugin/Action/EntityCreate.php) action doesn't contain the context annotations:

```php
/**
 * Provides a generic 'Create a new entity' action.
 *
 * @Action(
 *   id = "rules_entity_create",
 *   deriver = "Drupal\rules\Plugin\Action\EntityCreateDeriver",
 * )
 */
class EntityCreate extends RulesActionBase implements ContainerFactoryPluginInterface {
```

Instead it references the [EntityCreateDeriver](https://github.com/fago/rules/blob/8.x-3.x/src/Plugin/Action/EntityCreateDeriver.php) who based on all entity types, provides derivatives.

```php
  /**
   * {@inheritdoc}
   */
  public function getDerivativeDefinitions($base_plugin_definition) {
    foreach ($this->entityManager->getDefinitions() as $entity_type_id => $entity_type) {
      // Only allow content entities and ignore configuration entities.
      if (!$entity_type instanceof ContentEntityTypeInterface) {
        continue;
      }
      $this->derivatives["entity:$entity_type_id"] = [
        'label' => $this->t('Create a new @entity_type', ['@entity_type' => $entity_type->getLowercaseLabel()]),
        'category' => $entity_type->getLabel(),
        'entity_type_id' => $entity_type_id,
        'context' => [],
        'provides' => [
          'entity' => new ContextDefinition("entity:$entity_type_id", $entity_type->getLabel()),
        ],
      ] + $base_plugin_definition;
      // Add a required context for the bundle key, and optional contexts for
      // other required base fields. This matches the storage create() behavior,
      // where only the bundle requirement is enforced.
      $bundle_key = $entity_type->getKey('bundle');
      $base_field_definitions = $this->entityManager->getBaseFieldDefinitions($entity_type_id);
      foreach ($base_field_definitions as $field_name => $definition) {
        if ($field_name != $bundle_key && !$definition->isRequired()) {
          continue;
        }
        $required = ($field_name == $bundle_key);
        $multiple = ($definition->getCardinality() === 1) ? FALSE : TRUE;
        $this->derivatives["entity:$entity_type_id"]['context'][$field_name] = new ContextDefinition(
          $definition->getType(), $definition->getLabel(), $required, $multiple, $definition->getDescription()
        );
      }
    }
    return $this->derivatives;
  }
```

Also see the [related ticket](https://www.drupal.org/node/2409055).
