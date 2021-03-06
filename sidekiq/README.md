# Sidekiq as a service using Upstart

Manage multiple Sidekiq servers as services on the same box using Ubuntu upstart.

## Installation

    # Copy the scripts to services directory
    sudo cp sidekiq.conf sidekiq-manager.conf /etc/init

    # Create an empty configuration file
    sudo touch /etc/sidekiq.conf

## Before starting...

You need to customise `sidekiq.conf` to:

* Set the right user your app should be running on unless you want root to execute it!
  * Look for `setuid nobody` and `setgid nobody`, replace `nobody` to whatever your deployment user is.

## Managing

Sidekiq apps are referenced in /etc/sidekiq.conf by default. Add each app's path as a new line, e.g.:

```
/home/apps/my-cool-ruby-app
/home/apps/another-app/current
```

Start:

`sudo start sidekiq-manager`

This script will run at boot time.

Start a single:

`sudo start sidekiq app=/path/to/app`

## Logs

Everything is logged by upstart, defaulting to `/var/log/upstart`.

Each sidekiq instance is named after its directory, so for an app called `/home/apps/my-app` the log file would be `/var/log/upstart/sidekiq-_home_apps_my-app.log`.

## Conventions

* a config file to exist under `config/sidekiq.yml` in your app. E.g.: `/home/apps/my-app/config/sidekiq.yml`.
