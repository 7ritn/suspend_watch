# suspend_watch

Python script for monitoring and suspending the computer if no activity is detected

## Requirements

The project uses:

* PyYAML - for the config
* psutil - for matching process names
* pytimeparse - for easier time specifications within the config file
* conntrack-tools - backend for network activity checks
* (systemd) - per default uses systemd to suspend computer, but suspend command is customizeable

## Usage

The script needs to be executed with elevated permissions for conntrack and systemd. Be aware that the suspend_command is executed with elevated privileges. Thus, the config file needs to be only editable by root. Per default the script will not work with a world-writeable file, but this can be overwritten with `--unsafe`

CLI Options:

```
usage: Suspend Watch [-h] [-n] [--unsafe] [-c] [-v VERBOSITY] [-t TIMEOUT] [-f FREQUENCY] [--suspend_command SUSPEND_COMMAND] [--conntrack_path CONNTRACK_PATH] config

positional arguments:
  config                config file containing checks

optional arguments:
  -h, --help            show this help message and exit
  -n, --dryrun          don't actually suspend (default False)
  --unsafe              don't check if config is world-writeable (default False)
  -c, --check           only perform one check (default False)
  -s, --ignore_stale    ignore stale connections (default False)
  --fast_resuspend      after being woken up, resuspend if no activity is detected at an initial check after 15s (default False)
  -v VERBOSITY, --verbosity VERBOSITY
                        increase output verbosity (default 0)
  -t TIMEOUT, --timeout TIMEOUT
                        duration of no activity until suspend_command is executed (default 10 min)
  -f FREQUENCY, --frequency FREQUENCY
                        frequency of checks in seconds (default 1 min)
  --suspend_command SUSPEND_COMMAND
                        executeable to be run when no activity detected (default /usr/bin/systemctl suspend)
  --conntrack_path CONNTRACK_PATH
                        path to conntrack executeable (default /usr/bin/conntrack)
```

Checks defined in a config file. Example with explanation in `example.conf`.
