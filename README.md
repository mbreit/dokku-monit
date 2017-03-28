# dokku monit (in development, *unstable*)

Dokku plugin which provides health checks by monitoring and
restarting apps with Monit.

## Requirements

This plugin is developed and tested on Ubuntu 16.04 and Dokku 0.8.2.
It should work on other distributions but requires systemd.
Pull requests to support other init systems such as upstart
for Ubuntu 14.04 are of course welcome.

Monit has to be installed (`apt-get install monit`), but does not have
to be started/enabled. This plugin will start its own Monit instance
running as the `dokku` user. This way you can use the system wide Monit
configuration however you like or disable it with
`systemctl disable monit`.

## Installation

```shell
sudo apt-get install monit
sudo dokku plugin:install https://github.com/mbreit/dokku-monit.git
```

## Usage

Show all available commands:

```shell
dokku monit
```

Enable monit for an app:

```shell
dokku monit:enable myapp
```

This will send an http request to the application every two minutes.
When it fails two times, the app will be restarted.

To remove the app from Monit:

```shell
dokku monit:disable myapp
```

If you want do temporarily disable and enable monitoring, you can use:

```shell
dokku monit:unmonitor myapp
dokku monit:monitor myapp
```

You can also use `monit:unmonitorall` and `monit:monitorall`
to disable all apps at once.

This keeps the Monit configuration but disables the check with
Monit.

Show the Monit status for one app or for all apps:

```shell
dokku monit:status myapp
dokku monit:statusall
```

The monit logs can be shown with (requires syslog to be configured
to log to journald, which is the default on Ubuntu 16.04):

```shell
dokku monit:logs
dokku monit:logs -f
```

## Configuration

You can configure Monit in the `.monitrc` file in the Dokku home directory.
See https://mmonit.com/monit/documentation/monit.html for details.

It is important to keep the `include /home/dokku/*/monitrc` line,
because this is used to include the configuration for the individual
Dokku apps. Everything else should be configurable without breaking
the functionality of this plugin. This file does not get overwritten
when updating this plugin.

For configuring the individual app checks, you can set the
following configuration variables (use `--no-restart`):

| Variable                   | Default | Description                                              |
| ---                        |     --- | ---                                                      |
| `MONIT_CONTENT`            |         | Expected content in HTTP response                        |
| `MONIT_REQUEST`            |     `/` | Request path, eg. `/status`                              |
| `MONIT_ALERT`              |         | Mail address to notify on state changes                  |
| `MONIT_RESTART_TIMEOUT`    |     120 | Timeout for `dokku ps:restart` in seconds                |
| `MONIT_RESTART_CYCLES`     |       2 | Number of failed checks before restarting the app        |
| `MONIT_UNMONITOR_RESTARTS` |       5 | Restart Limit: Number of restarts before unmonitoring    |
| `MONIT_UNMONITOR_CYCLES`   |      20 | Restart Limit: Interval for the above number of restarts |

For example if you want to receive alerts by mail, set

```shell
dokku config:set --no-restart myapp MONIT_ALERT=myaddress@example.com
```

You can also set a global alert mail address in the
global Monit configuration in `~dokku/.monitrc`.

## License

This project is released under the MIT License. See the
[LICENSE](LICENSE) file for details.
