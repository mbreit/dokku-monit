# dokku monit (in development, *unstable*)

Dokku plugin to monitor and restart failing apps with Monit.

## Requirements

This plugin is developed and tested on Ubuntu 16.04 and Dokku 0.8.2.
Monit has to be installed (`apt-get install monit`), but does not have
to be started/enabled. This plugin will start its own monit instance
running as the dokku user. This way you can use the system wide monit
configuration however you like or disable is with
`systemctl disable monit`.

## Installation

```shell
sudo dokku plugin:install https://github.com/mbreit/dokku-monit.git
```

## Usage

You need to configure this plugin with `dokku config:set` for the
apps you want to monitor.
*This is not ideal and might change in future versions.*

To enable monit for an app run:

```shell
dokku config:set --no-restart myapp MONIT_ENABLED=true
```

This will send an http request to the application every two minutes.
When it fails two times, the app will be restarted.

## Configuration

The following configuration variables can be set:

* `MONIT_ENABLED`: Enable monitoring for one app, unset or set to empty string to disable
* `MONIT_CONTENT`: Expected content in HTTP response
* `MONIT_REQUEST`: Request path, eg. `/status`
* `MONIT_ALERT`: Mail address to notify on state changes
