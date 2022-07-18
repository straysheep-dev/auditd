[![Actively Maintained](https://img.shields.io/badge/Maintenance%20Level-Actively%20Maintained-green.svg)](https://gist.github.com/cheerfulstoic/d107229326a01ff0f333a1d3476e068d)

        ___             ___ __      __
       /   | __  ______/ (_) /_____/ /
      / /| |/ / / / __  / / __/ __  / 
     / ___ / /_/ / /_/ / / /_/ /_/ /  
    /_/  |_\__,_/\__,_/_/\__/\__,_/   

Best Practice Auditd Configuration

## Idea

The idea of this auditd configuration is to provide a basic configuration that

- works out-of-the-box on all major Linux distributions 
- fits most use cases
- produces a reasonable amount of log data
- covers security relevant activity
- is easy to read (different sections, many comments)

## About this fork

This fork is an attempt at merging this project with the work done here: <https://github.com/bfuzzy1/auditd-attack> in mapping rule keys to MITRE ATT&ACK id's.

The keys in this configuration have both the id and name in the key, for example:
```bash
-k T1547.006_Kernel_Modules
```

Aside from changing keys to id's, and adding any missing and compatible rules found in the other project, nearly everything was left the same as it is in the upstream version where possible to make tracking changes and updates easier.

The other major difference is this version is meant to work with the [built in rule files that ship with auditd](https://github.com/linux-audit/audit-userspace/tree/master/rules). 

audit.rules was renamed to 40-mitre.rules (see [40-local.rules](https://github.com/linux-audit/audit-userspace/blob/master/rules/40-local.rules)).

That also means these lines:

```
# Remove any existing rules
-D

# Buffer Size
## Feel free to increase this if the machine panic's
-b 8192

# Failure Mode
## Possible values: 0 (silent), 1 (printk, print a failure message), 2 (panic, halt the system)
-f 1

# Ignore errors
## e.g. caused by users or files not found in the local environment
-i
```

and these lines:
```
# Make The Configuration Immutable --------------------------------------------

##-e 2
```

Occur in [10-base-config.rules](https://github.com/linux-audit/audit-userspace/blob/master/rules/10-base-config.rules) and [99-finalize.rules](https://github.com/linux-audit/audit-userspace/blob/master/rules/99-finalize.rules) instead.

This was done in case a system is already using built in rules, or has other custom rules running as well, such as any of the built in 30-*.rules ([nispom](https://github.com/linux-audit/audit-userspace/blob/master/rules/30-nispom.rules), [ospp](https://github.com/linux-audit/audit-userspace/blob/master/rules/30-ospp-v42.rules), [pci](https://github.com/linux-audit/audit-userspace/blob/master/rules/30-pci-dss-v31.rules), [stig](https://github.com/linux-audit/audit-userspace/blob/master/rules/30-stig.rules))

This is a **work in progress** 

Rule mappings will likely need revised, and errors / incompatibilities / unnecessary rules removed.

### Additional License Information

[bfuzzy1's auditd-attack](https://github.com/bfuzzy1/auditd-attack) is released under the [MIT license](https://github.com/bfuzzy1/auditd-attack/blob/master/auditd-attack/LICENSE).

## Sources

The configuration is based on the following sources

Gov.uk auditd rules
https://github.com/gds-operations/puppet-auditd/pull/1

CentOS 7 hardening
https://highon.coffee/blog/security-harden-centos-7/#auditd---audit-daemon

Linux audit repo 
https://github.com/linux-audit/audit-userspace/tree/master/rules

Auditd high performance linux auditing
https://linux-audit.com/tuning-auditd-high-performance-linux-auditing/

### Further rules

Not all of these rules have been included. 

For PCI DSS compliance see: 
https://github.com/linux-audit/audit-userspace/blob/master/rules/30-pci-dss-v31.rules

For NISPOM compliance see:
https://github.com/linux-audit/audit-userspace/blob/master/rules/30-nispom.rules

## Video Explanations by IppSec

IppSec captured a video that explains how to detect the exploitation of the OMIGOD vulnerability using auditd. In that video, he walks you through the audit configuration maintained in this repo and explains how to use it. I highly recommend this video to get a better understanding of what is happening in the config.

https://www.youtube.com/watch?v=lc1i9h1GyMA

## Contribution

Please contribute your changes as pull requests
