# ep-3 Bookingsystem

The ep-3 Bookingsystem is an open source (MIT licensed) web application to enable users to check and book free places of
an arbitrary facility easily online via one huge calendar.

It was initially developed to enable booking free squares of a covered court for a tennis club, improved along some
versions, tried to offer commercially as a SaaS - and finally released as open source software.

Among its primary features are extensive customization capabilities (thus making it interesting even outside the tennis
branch), multilingualism (currently shipped with english and german), an interactive, easy-to-use calendar, an
easy-to-use and easy-to-understand backend, a consistent and clear visual design and a fully responsive layout (thus
looking nice on mobile devices as well).

More features may be explored via our website (http://bs.hbsys.de/) or simply by downloading and trying the system
yourself.

## Documentation and installation

Documentation and installation instructions can be found in the following directory:

```
data/docs/
```

## Architecture

The system is based on the well-known LAMP stack (Linux, Apache 2+, MySQL 5+, PHP 5.4+) and the popular and powerful
[Zend Framework 2](http://framework.zend.com/) (2.4+).

Dependencies are managed with [composer](https://getcomposer.org/).

The source code is version controlled with [Git](http://git-scm.com/) and hosted at [GitHub](https://github.com/).

The link to the GitHub repository is

```
https://github.com/tkrebs/ep3-bs
```

where you can find stable and (latest) development releases.

## Version

The current version (June, 2018) is 1.6.3.

Version 1.6.3 introduced some GDPR compliance based changes and requested features.

Version 1.6.2 changed the configuration behaviour and requires some manual changes (see data/docs/update.txt). Otherwise, the update will not work.

Version 1.6 introduced some requested features and fixed quite some bugs. It also introduced better support for custom translations and modules.

Version 1.5 introduced some requested features (billing administration, custom billing statuses and colors) and fixed some bugs.

Version 1.4 introduced some requested features and the latest third party libraries and frameworks.

## Bug reports, feature requests, ideas ...

We use the GitHub Issue Tracker for such things:

https://github.com/tkrebs/ep3-bs/issues

# Payment

## paypal
create an account at paypal.com - first sandbox for developing later live - get the NVP/SOAP credentials (username,password,signature) and put them in your config/autoload/local.php

## stripe
create an account at stripe.com - get the API keys (publishable and secret key) - first test later live - and put them in your config/autoload/local.php

## apple pay via stripe
verify your domian for apple pay
```
https://stripe.com/docs/stripe-js/elements/payment-request-button#verifying-your-domain-with-apple-pay
```

## removing unpaid booking try's
cancelling bookings is not allowed in our version - so we remove unpaid user online bookings automatically if they are not completed during the payment process - we remove that bookings after 3 hours (the standard lifetime of a paypal token) in the db with folowing sql
```
DROP EVENT remove_unpaid_bookings;
SET GLOBAL event_scheduler = ON;
CREATE EVENT remove_unpaid_bookings ON SCHEDULE EVERY 10 MINUTE STARTS '2019-11-14 00:00:00' ON COMPLETION PRESERVE DO delete from bs_bookings where `status` = 'single' and `status_billing` = 'pending' and created < (NOW() - INTERVAL 3 HOUR) and bid in (select bid from bs_bookings_meta where `key` = 'directpay' and `value` = 'true');
```




