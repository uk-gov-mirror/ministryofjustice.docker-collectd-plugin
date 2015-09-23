# docker-collectd-plugin

A [Docker](http://docker.io) plugin for [collectd](http://collectd.org)
using [docker-py](https://github.com/docker/docker-py) and collectd's
[Python plugin](http://collectd.org/documentation/manpages/collectd-python.5.shtml).

This uses the new stats API [introduced by
\#9984](https://github.com/docker/docker/pull/9984) in Docker 1.5.

The following container stats are gathered for each container, with
examples of some of the metric names reported:

* Network bandwidth:
 * `network.usage.rx_bytes`
 * `network.usage.tx_bytes`
 * ...
* Memory usage:
 * `memory.usage.total`
 * `memory.usage.limit`
 * ...
* CPU usage:
 * `cpu.usage.total`
 * `cpu.usage.system`
 * ...
* Block IO:
 * `blockio.io_service_bytes_recursive-<major>-<minor>.read`
 * `blockio.io_service_bytes_recursive-<major>-<minor>.write`
 * ...

All metrics are reported with a `plugin:docker` dimension, and the name
of the container is used for the `plugin_instance` dimension.

## Install

1. Checkout this repository somewhere on your system accessible by
   collectd; for example as
   `/usr/share/collectd/docker-collectd-plugin`.
1. Install the Python requirements with `pip install -r
   requirements.txt`.
1. Configure the plugin (see below).
1. Restart collectd.

## Configuration

Add the following to your collectd config:

```
TypesDB "/usr/share/collectd/docker-collectd-plugin/dockerplugin.db"
LoadPlugin python

<Plugin python>
  ModulePath "/usr/share/collectd/docker-collectd-plugin"
  Import "dockerplugin"

  <Module dockerplugin>
    BaseURL "unix://var/run/docker.sock"
    Timeout 3
  </Module>
</Plugin>
```

## Requirements

* docker-py
* python-dateutil
* docker 1.5+
