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

To see all available commands run:

```shell
dokku monit
```

To enable monit for an app run:

```shell
dokku monit:enable myapp
```

This will send an http request to the application every two minutes.
When it fails two times, the app will be restarted.

To remove the app from Monit, run:

```shell
dokku monit:disable myapp
```

If you want do temporarily disable and enable monitoring, you can use:

```shell
dokku monit:unmonitor myapp
dokku monit:monitor myapp
```

This keeps the Monit configuration but disables the check with
Monit.

To show the apps Monit status, run one of:

```shell
dokku monit:status myapp
dokku monit:statusall
```

## Configuration

The following configuration variables can be set:

* `MONIT_CONTENT`: Expected content in HTTP response
* `MONIT_REQUEST`: Request path, eg. `/status`
* `MONIT_ALERT`: Mail address to notify on state changes

## License

This project is released under the MIT License. See the
[LICENSE](LICENSE) file for details.
