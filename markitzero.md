## Overview 
Generall speaking these are the Network ranges we use:<br />
| Network       | CIDR         |
|:-------------:|:------------:|
| VM DR Network | (10.198.1.x) |
| VM Network    | (10.98.x.x)  |

1. Create [DNS](https://confluence.ta.com/pages/viewpage.action?pageId=37163265) entries
1. Go to [VMware](https://vchq1.ta.com)
    1. Change to the `VMs and Templates` view
    1. Exand folder structure until you get to `Templates`: _E.g._ [TADR > Linux > Templates]
      1. _Right click_ on template of choice: _E.g._ **_OL7_2DISK_DOCKER**
      1. Select `New VM from Template...`
            + **Select a name and folder**: _E.g._ [TADR > Linux > Docker > QA]
            + **Select a computer resource**: _E.g._ [TADR > DR_Cluster1]
            + **Select storage**: _E.g._ `Same format as source` & `Datastore Default` [ drlinux_ds_cluster_1 | drwin_ds_cluster_1]
            + **Select clone options**: 
            + **Check**: _"Customize this virtual machine's hardware"_
                + Ensure all your disk drives are here
            + **Customize hardware**: _E.g._ Change Memory to 8GB
1. Power On VM
    1. Go to console
    1. Press _tab_ on the "**Install Oracle Linux 7**" screen to interrupt boot process and append the following: (_You only have 60s_)
        + `net.ifnames=0` 
        + `ks=http://spacewalk1.ta.com/ks/cfg/org/1/label/vmware-guest-ol7-2disk-docker` 
            + The kickstart script will prompt you for a `hostname`; Leave the other networking information set to their defaults
1. Go into [Spacewalk](https://spacewalk1.ta.com)
    1. Search by "**Systems**" for your `hostname` in the top _search_ bar
    1. Click your `host`, and navigate to the "**Groups**" tab
    1. Remove "**New Servers**" from "_Group Name_" section by _checking_ it and _clicking_ **"Leave Selected Groups"**.
    1. **Join** your `host` to the appropriate `Tier` - [Development | Management | Production | QA]
        * You should have one of each of the `Group Names` - [OS, Role, Tier]
1. Go into [Puppet](http://puppet2.ta.com/#/configure/certificates) 
    1. Select `Unsigned certs` from **Setup** section
    1. Accept the certificate for your `host`
1. Go into [Zabbix](https://zabbix3.ta.com/zabbix/hosts.php)<br />
This step is contingent upon `Puppet` running, and adding the `host` to _Zabbix_ itself.
    1. Select your `host` from **Configuration** > **Hosts** section
    1. Adjust _In groups_ as necessary: _E.g._ Remove `Linux`; Add `Linux/Docker`
1. Log into _console_ run as root '`eject`'
    1. _Right click_ on VM and select `Edit settings...`
        1. **Uncheck**: _Connected_ box next to `CD/DVD drive 1`
        1. Change from **Content Library ISO File** to **Client Device** and click "OK"
1. Reboot
