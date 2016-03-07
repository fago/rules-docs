# Executing the automated tests

This module comes with PHPUnit and SimpleTest tests. You need a working Drupal 8
installation and a checkout of the Rules module in the modules folder.

#### PHPUnit

Be sure to update the values of SIMPLETEST_DB and SIMPLETEST_BASE_URL as suiting for your environment and run:

    export SIMPLETEST_DB=mysql://drupal-8:password@localhost/drupal-8
    export SIMPLETEST_BASE_URL=http://drupal-8.localhost
    ./vendor/phpunit/phpunit/phpunit -c ./core/phpunit.xml.dist ./modules/rules

#### Simpletest

Rules 8.x-3.x does not make use of the Simpletest framework any more, but uses solely PHPUnit based tests. However, you can still run the PHPUnit based tests via the Simpletest framework of Drupal 8:

    drush en -y simpletest rules

And then run the tests

    drush test-run 'Rules, Rules conditions'

    php ./core/scripts/run-tests.sh --verbose --color "rules"

You can also execute the test cases from the web interface at ``/admin/config/development/testing``.
