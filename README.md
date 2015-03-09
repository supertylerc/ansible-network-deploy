# ansible-network-deploy

A set of playbooks and a role designed to setup OSPF across multiple vendors.

## Usage

### Prerequisites

You will need the following software, along with all prerequisites that go
along with them:

* [ansible-junos-stdlib](https://github.com/Juniper/ansible-junos-stdlib)
* [ansible-netmiko-stdlib](https://github.com/supertylerc/ansible-netmiko-stdlib)
* [Ansible](https://github.com/ansible/ansible)

Additionally, you will need password-based SSH access to any non-Cumulus and
non-Junos devices.  Key-based access is used with Juniper and Cumulus, but
it is not currently tested with the **ansible-netmiko-stdlib** module(s).  You
should have a user named "ansible" on all of the devices--this is the user
that will connect to and configure the devices.

If you are testing Juniper devices, ensure that the devices are configured
for NETCONF over SSH.

For Cisco IOS devices, the `ansible` user must be configured to enter `enable`
mode by default.  In other words, the module does not enter "`enable`" for you.

Your devices should be connected to each other and fully configured--minus
OSPF.  Please do not configure OSPF.  This repository specifically configures
OSPF.

### Configuring

You will need to update the following files:

* `./hosts`
* `./host_vars/${hostname}.yml`

`./hosts` should contain FQDNs.  You can change the group names with no change
to the functionality of this system.  These names should be resolvable.

You should use the `./host_vars/${hostname}.yml` files as examples for how to
define your device's OSPF configuration.  You should create files to match
your resolvable FQDNs in `./hosts`.

### Running

`ansible-playbook -i hosts deploy.yml`

### Supported Vendors

The following vendors and operating systems have been tested:

* Juniper - Junos 12.1X47 on vSRX
* Cisco - IOS-XE 03.14.01S/IOS 15.5(1)S1 on CSR1000v
* Cumulus - Cumulus Linux 2.5

Because this system configures Quagga for Cumulus, it should also be compatible
with any Linux distribution running up-to-date Quagga.

Because this system configures IOS with the `ansible-netmiko-stdlib` module,
it can potentially work for any vendor that
[Netmiko](https://github.com/ktbyers/netmiko) supports.  However, you would
need to update the vendors, modularize the `device_type` parameter, and create
the relevant templates in `./roles/common/templates/${vendor}/router_id.j2` and
`./roles/common/templates/${vendor}/ospf.j2`.

### Caveats

Although this system can support every vendor that Netmiko supports, you should
take care to ensure that the templates are correct.  In addition, utilizing the
diffs ability of the `ansible-netmiko-stdlib` module requires that the device
display it's full current configuration from the default login mode with the
`show running-config` command.  Finally, this system requires that you already
be in "enable" mode (privileged exec).  This will eventually be corrected to
support enable passwords, but for now your IOS user can be configured with
`username ansible privilege 15`.

## Support

Please open GitHub issues with any problems or questions you run into.

## Author

Tyler Christiansen

## Contributing

See Support above.  Submit pull requests if you want to do more than just open
issues.

## License

MIT license.  Don't blame me if your production networks break.  I'm not
liable for how you use this.  It would be nice if you dropped me an email
at **mail at tylerc dot me** to let me know you thought this was cool and/or
helpful.  If you use it in production, I'm even more interested.
