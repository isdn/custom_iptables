#### Systemd unit to manage separate sets of rules in iptables.

Basically, the [custom_iptables@.service](etc/systemd/system/custom_iptables@.service) file is a template unit file.
So, a necessary set name can be passed as an argument after `@`.

As an example, I created two files in `/etc/iptables/`:

[base.rules](etc/iptables/base.rules)
[base.flush.rules](etc/iptables/base.flush.rules)

A `*.rules` file is a file in the "iptables-restore" format and used to add custom rules.
A `*.flush.rules` file is a file in the same format used to restore previous state.
Both are required for the same set. The `.rules` file is loaded on start, and `.flush.rules` on stop.

In my example two essential chains are created - `custom-input` and `custom-output`, attached to `INPUT` and `OUTPUT` respectively.
Other chains and rules are attached to them.


##### Installation

Download and copy the files to specified locations.
After that:

```bash
systemctl daemon-reload
systemctl enable custom_iptables@base.service
systemctl start custom_iptables@base.service
```

Other sets can be added to `/etc/iptables/`.
For example, `/etc/iptables/other.rules` and `/etc/iptables/other.flush.rules`.
After that, a new unit can be started:

```bash
systemctl enable custom_iptables@other.service
systemctl start custom_iptables@other.service
```


