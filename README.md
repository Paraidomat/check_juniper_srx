# check_juniper_srx
A Nagios compliant check for Juniper SRX Firewalls.

# Setup

Just download the `.py` file from this repository and save it to a folder where your monitoring system can access it.

## Requirements

The script uses the following Python 3 Modules:
* `sys`
* `getopt`
* `ipaddress`
* `re`
* `pysnmp`

# Usage

Simply call the Python-Skript:

```
check_juniper_srx -i <ipv4Address> -c <communityString> -m <mode> [-n <interface-name>]
```

## Available Modes:

* `cp_sessions`
* `flow_sessions`
* `cpu_load_re`
* `cpu_load_fpc`
* `memory_fpc`
* `memory_re`
* `temperature`
* `interface_status`
* `interface_status_detail`
* `interface_list`

## Example Implementation for Icinga 2

### CheckCommand

```
object CheckCommand "check_juniper_srx" {
        import "plugin-check-command"
        command = [ PluginDir + "check_juniper_srx" ]
        arguments = {
            "-i" = "$check_juniper_srx_address$"
            "-c" = "$check_juniper_srx_community$"
            "-m" = "$check_juniper_srx_mode$"
        }

        vars.check_juniper_srx_address = "$address$"
        vars.check_juniper_srx_community = "public"
}

```
### Service object

```
apply Service "Juniper SRX FPC-CPU-Load" {
        import "normal-service"
        check_command = "check_juniper_srx"
        vars.check_juniper_srx_mode = "cpu_load_fpc"

        assign where match ( "*SRX*", host.vars.type)
        vars.servicegroup = "CPULoad"
}
```
