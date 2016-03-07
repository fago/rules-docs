# Events

Rules uses the [Symfony event dispatching system](http://symfony.com/doc/current/components/event_dispatcher/introduction.html) to trigger events and invoke reaction rules when an event occurs. A module that wants to provide events can do so without having a module dependency to Rules -dispatching standard Symfony events in code is enough.

In order to make an event known to Rules a *.rules.events.yml file has to be
provided to register the event(s). Example from Rules itself:

```yaml
rules_user_login:
  label: 'User has logged in'
  category: 'User'
  context:
    account:
      type: 'entity:user'
      label: 'Logged in user'
```

This entry registers an event called "rules_user_login" which has one context parameter called "account".

Next, an event class should be declared (you can also use Symfony's
```GenericEvent``` directly, but an explicit class is encouraged):

```php
/**
 * Event that is fired when a user logs in.
 *
 * @see rules_user_login()
 */
class UserLoginEvent extends Event {

  const EVENT_NAME = 'rules_user_login';

  /**
   * The user account.
   *
   * @var \Drupal\user\UserInterface
   */
  public $account;

  /**
   * Constructs the object.
   *
   * @param \Drupal\user\UserInterface $account
   *   The account of the user logged in.
   */
  public function __construct(UserInterface $account) {
    $this->account = $account;
  }

}
```
The values for the specified context have to be available either as public properties or via a getter method that is named "get{{ CONTEXT_NAME }}".

Invoking or dispatching the event looks like this:

```php
  $event = new UserLoginEvent($account);
  $event_dispatcher = \Drupal::service('event_dispatcher');
  $event_dispatcher->dispatch(UserLoginEvent::EVENT_NAME, $event);
```

An instance of the ```UserLoginEvent``` class is created, passing along the
account user object as context parameter. The event dispatching service is used
to invoke all event subscribers. Rules itself is among those subscribers with
its ```GenericEventSubscriber``` class which will trigger all reaction rules
that are configured for the event.

Note: Do not use ```\Drupal``` when invoking events from within a class, use
[dependency injection](https://www.drupal.org/node/2133171) for the event dispatcher service instead.

