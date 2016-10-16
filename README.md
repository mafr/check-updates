# check-updates

This is a Nagios/Icinga plugin to check for security updates on a
Debian/Ubuntu machine. It uses the `update-notifier-common` package
and relies on an up to date package index.

## Prerequisites

The script requires a Python interpreter. It has been tested on Python 2.7,
3.4, and 3.5 but earlier versions of Python 3 may also work.

It also requires the `update-notifier-common` package and an up to date
package index, for example by enabling package list downloads in
`/etc/apt/apt.conf.d/10periodic`:

```
APT::Periodic::Update-Package-Lists "1";
```

Behind the scenes, this uses `cron.daily` to run `apt-get update` once
per day. The number indicates the update interval in days.

## Usage

The check does not require any command line parameters. Example:

```
$ ./check_updates 
OK - no security updates available (7 regular updates).
$
```

In case there are security updates available, the script issues a `WARNING`
response:

```
$ ./check_updates 
WARNING - 3 security updates ready for installation.
$
```

If the package index is outdated, the check returns `CRITICAL`. Missing
files or infrastructure leads to an `UNKNOWN` response with a message
pointing to the reason.
