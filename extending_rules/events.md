# Events

Rules uses the (Symfony event dispatching system)[http://symfony.com/doc/current/components/event_dispatcher/introduction.html]
to trigger events and invoke reaction rules when an event occurs. A module that
wants to provide events can do so without having a module dependency to Rules -
dispatching standard Symfony events in code is enough.

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

This entry registers an event called "rules_user_login" which has one context
parameter called "account".

Next, an event class should be declared (you can also use Symfony's
```GenericEvent``` directly, but an explicit class is encouraged):

```php
class UserLoginEvent extends GenericEvent {

  const EVENT_NAME = 'rules_user_login';

}
```

Invoking the event looks like this:

```php
// Set the account twise on the event: as the main subject but also in the
// list of arguments.
$event = new UserLoginEvent($account, ['account' => $account]);
$event_dispatcher = \Drupal::service('event_dispatcher');
$event_dispatcher->dispatch(UserLoginEvent::EVENT_NAME, $event);
```

An instance of the ```UserLoginEvent``` class is created, passing along the
account user object as context parameter. The event dispatching service is used
to invoke all event subscribers. Rules itself is among those subscribers with
its ```GenericEventSubscriber``` class which will trigger all reaction rules
that are configured for the event.

Note: Do not use ```\Drupal``` when invoking events from within a class, use
(dependency injection)[https://www.drupal.org/node/2133171] for the event
dispatcher service instead.

Note 2: Make sure to issue a service container rebuild or
(cache clear)[https://www.drupal.org/documentation/clearing-rebuilding-cache]
when you configure and save new reaction rules, so that the event registration
is picked up.
