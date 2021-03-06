# Introduction

Magui is a wrapper that calls functions from the Python library of Citellus [README.md](README.md).

Some problems are not detected only one one node, but are made by the aggregation of data across them, for example:

- ntp sync
- galera replication status
- etc
- RHEL release differences

Magui aims to use Citellus for gathering the data and later, write plugins to analyze that information.

## Highlights
- Reuse saved citellus.json to speed up analisys on several files, retrigger if inconsistencies

- Plugins use uuid to identify plugin properly and act on them.

- Allows to get data from remote hosts with ansible-playbook

Check latest changes on <Changelog.md>

## Usage help
Plugins for Magui are to be written in Python, check next section for details.

```
usage: magui.py [arguments] [-h] [-d {INFO,DEBUG,WARNING,ERROR,CRITICAL}]
                            [--list-plugins] [--description] [-m MPATH]
                            [--output FILENAME] [--run] [--hosts hosts] [-q]
                            [-i SUBSTRING] [-x SUBSTRING] [-p [0-1000]]
                            [-mf MFILTER]
                            [sosreports [sosreports ...]]

Processes several generic archives/sosreports scripts in a uniform way, to
interpret status that depend on several systems data

positional arguments:
  sosreports

optional arguments:
  -h, --help            show this help message and exit
  -d {INFO,DEBUG,WARNING,ERROR,CRITICAL}, --loglevel {INFO,DEBUG,WARNING,ERROR,CRITICAL}
                        Set log level
  --list-plugins        Print a list of discovered Magui plugins and exit
  --description         With list-plugins, also outputs plugin description
  -m MPATH, --mpath MPATH
                        Set path for Magui plugin location if not default
  --output FILENAME, -o FILENAME
                        Write results to JSON file FILENAME
  --run, -r             Force run of citellus instead of reading existing
                        'citellus.json'
  --hosts hosts         Gather data via ansible from remote hosts to process.

Filtering options:
  -q, --quiet           Enable quiet mode
  -i SUBSTRING, --include SUBSTRING
                        Only include plugins that contain substring
  -x SUBSTRING, --exclude SUBSTRING
                        Exclude plugins that contain substring
  -p [0-1000], --prio [0-1000]
                        Only include plugins are equal or above specified prio
  -mf MFILTER, --mfilter MFILTER
                        Only include Magui plugins that contains in full path
                        that substring
```

### Running a check

This is an example of execution of Magui against a set of sosreports with `seqno` plugin of Citellus enabled.

~~~sh
#magui.py * -i seqno # (filtering for ‘seqno’ plugins.
{'/home/remote/piranzo/citellus/citellus/plugins/openstack/mysql/seqno.sh': {'ctrl0.localdomain': {'err': '08a94e67-bae0-11e6-8239-9a6188749d23:36117633\n',
                                                                                                   'out': '',
                                                                                                   'rc': 0},
                                                                             'ctrl1.localdomain': {'err': '08a94e67-bae0-11e6-8239-9a6188749d23:36117633\n',
                                                                                                   'out': '',
                                                                                                   'rc': 0},
                                                                             'ctrl2.localdomain': {'err': '08a94e67-bae0-11e6-8239-9a6188749d23:36117633\n',
                                                                                                   'out': '',
                                                                                                   'rc': 0}}}

On this example, UUID and SEQNO is shown for each controller.
~~~

#### Running a check against remote hosts

~~~sh
# Prepare host list in ansible-style
echo "host1" > hosts
echo "host2" >> hostsfile

# Run magui against them
./magui.py --hosts hostsfile
~~~


# Plugin development for Magui

Please do check <doc/magui-plugin-development.md> for more details.

Please if you have any idea on any improvements please do not hesitate to open an issue or submit your contributions.
