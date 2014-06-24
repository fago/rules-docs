# Executing the automated tests

This module comes with PHPUnit and Simpletest tests. You need a working Drupal 8
installation and a checkout of the Rules module in the modules folder.

#### PHPUnit

    cd /path/to/drupal-8/core
    ./vendor/bin/phpunit ../modules/rules

#### Simpletest using Drush

    drush test-run 'Rules, Rules conditions'

You can also execute the test cases from the web interface at
``/admin/config/development/testing``.
