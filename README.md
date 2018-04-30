# update-leap

This utility verifies, and if necessary updates the leap-second file used by NTP.

For current documentation, run leap-second -h

It is primarily a bash script; the utilities that it depends on are
listed in the help.

Copyright (C) 2014 Timothe Litt litt at acm dot org

This script may be freely copied, used and modified providing that
this notice and the copyright statement are included in all copies
and derivative works.  No warranty is offered, and use is entirely at
your own risk.  Bugfixes and improvements would be appreciated by the
author.

Below is the help file as of V1.004

````
Usage: update-leap [options] [leapfile]

Verifies and if necessary, updates leap-second definition file

All arguments are optional:  Default (or current value) shown:
    -s    Specify the URL of the master copy to download
          ftp://ftp.nist.gov/pub/time/leap-seconds.list
    -4    Use only IPv4
    -6    Use only IPv6
    -p 4|6
          Prefer IPv4 or IPv6 (as specified) addresses, but use either
    -d    Specify the filename on the local system

    -e    Specify how long before expiration the file is to be refreshed
          Units are required, e.g. "-e 60 days"  Note that larger values
          imply more frequent refreshes.
          "60 days"
    -f    Specify location of ntp.conf (used to make sure leapfile directive is
          present and to default  leapfile)
          /etc/ntp.conf
    -F    Force update even if current file is OK and not close to expiring.
    -c    Command to restart NTP after installing a new file
          <none> - ntpd checks file daily
    -r    Specify number of times to retry on get failure
          6
    -i    Specify number of minutes between retries
          10
    -l    Use syslog for output (Implied if CRONJOB is set)
    -L    Don't use syslog for output
    -P    Specify the syslog facility for logging
          daemon
    -t    Name of temporary file used in validation
          /tmp/leap-seconds.11817.tmp
    -q    Only report errors to stdout
    -v    Verbose output
    -z    Specify path for utilities
          /opt/sbin:/opt/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:
    -Z    Only use system path

update-leap will validate the file currently on the local system

Ordinarily, the file is found using the "leapfile" directive in /etc/ntp.conf.
However, an alternate location can be specified on the command line.

If the file does not exist, is not valid, has expired, or is expiring soon,
a new copy will be downloaded.  If the new copy validates, it is installed and
NTP is (optionally) restarted.

If the current file is acceptable, no download or restart occurs.

-c can also be used to invoke another script to perform administrative
functions, e.g. to copy the file to other local systems.

This can be run as a cron job.  As the file is rarely updated, and leap
seconds are announced at least one month in advance (usually longer), it
need not be run more frequently than about once every three weeks.

For cron-friendly behavior, define CRONJOB=1 in the crontab.

This script depends on wget logger tr sed shasum dos2unix

Version 1.004
````