ansible-iptables for internal & external network firewall configuration
=========

[![Build Status](https://travis-ci.org/ernestas-poskus/ansible-iptables.svg?branch=master)](https://travis-ci.org/ernestas-poskus/ansible-iptables)
[![RHEL](http://img.shields.io/badge/supports-RHEL-green.svg)](https://en.wikipedia.org/wiki/Red_Hat_Enterprise_Linux)
[![Fedora](http://img.shields.io/badge/supports-Fedora-green.svg)](https://en.wikipedia.org/wiki/Fedora_(operating_system))
[![Ubuntu](http://img.shields.io/badge/supports-Ubuntu-green.svg)](https://en.wikipedia.org/wiki/Ubuntu_(operating_system))
[![Debian](http://img.shields.io/badge/supports-Debian-green.svg)](https://en.wikipedia.org/wiki/Debian)
[![BSD License](http://img.shields.io/badge/license-BSD-blue.svg)](http://opensource.org/licenses/BSD-3-Clause)

Installation
------------

``` bash
ansible-galaxy install ernestas-poskus.iptables
```

Requirements
------------

None.

Dependencies
------------

None.


Role Variables
--------------

```yaml
# Default action for: :INPUT DROP
iptables_initial_input_action: DROP

# Default action for: :FORWARD ACCEPT
iptables_initial_forward_action: ACCEPT

# Default action for: :OUTPUT ACCEPT
iptables_initial_output_action: ACCEPT

# Allow loopback interface (localhost)
iptables_allow_loopback: true

# Allow established and related connections
iptables_allow_established_connections: true

# Allow DNS resolution
iptables_allow_dns: false

# Allow NTP traffic for time synchronization
iptables_allow_ntp: true

# Allow ICMP ping requests
iptables_allow_icmp: true

# Internal network supports list:
# - IP's
# - HOSTS (iptables_internal_ips_from_hosts should be enabled)
iptables_internal_network: []

# Internal resolve IP from given host
iptables_internal_ips_from_hosts: false

# Internal hosts interface
iptables_internal_hosts_interface: "{{ ansible_default_ipv4.interface }}"

# Internal hosts IP protocol: ipv4 / ipv6
iptables_internal_hosts_ip_protocol: 'ipv4'

##########################
# Internal network configuration
##########################
iptables_internal_ports:
  - '1024:65535'
  - '111' # Portmapper
  - '161:162' # Simple network management protocol (SNMP).
  - '22' # ssh

# Internal network protocols
iptables_internal_protocols:
  - 'tcp'
  - 'udp'

##########################
# External network configuration
##########################
iptables_external_ports:
  - '22' # ssh

# External network protocols
iptables_external_protocols:
  - 'tcp'

# Additional rules for iptables
iptables_additional_rules: []

# Log dropped packets
iptables_log_dropped_packets: false

# Log limit of dropped packets in minutes
iptables_log_dropped_limit: 15

# LOGGING log level of dropped packets
iptables_log_logging_level: 4
```

Example Playbook
----------------

> Making internal network from given hosts

```yaml
- name: Configuring iptables
  hosts: all
  sudo: yes
  roles:
    - role: ernestas-poskus.iptables
      iptables_internal_network: "{{ groups['all'] }}"
      iptables_internal_ips_from_hosts: true # when using hosts, this must be enabled
      iptables_log_dropped_packets: true
      iptables_log_dropped_limit: 1
```

> Making internal network from given IP's list

```yaml
- name: Configuring iptables
  hosts: all
  sudo: yes
  roles:
    - role: ernestas-poskus.iptables
      iptables_internal_network:
       - 192.168.0.1
       - 192.168.0.2
       - 192.168.0.3
      iptables_log_dropped_packets: true
      iptables_log_dropped_limit: 1
```

License
-------

Copyright (c) 2016, Ernestas Poskus
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.

* Neither the name of ansible-iptables nor the names of its
  contributors may be used to endorse or promote products derived from
  this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

Author Information
------------------

Twitter: @ernestas_poskus
