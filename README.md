aws-cgateway-role
=================

Sets up an Amazon Customer Gateway on a Debian-based machine

Requirements
------------

*N/A*

Role Variables
--------------

*To be completed. For now, see the [example playbook](#example-playbook).*

Dependencies
------------

*N/A*

Example Playbook
----------------

```yaml
---
- name: Setup network & routing services
  hosts: SERVERS-ROUTERS
  become: yes

  vars:
    cgw_bgp: true
    cgw_external_ip: 1.2.3.4
    cgw_config:
      - name: mickey
        vpc_subnet: 172.31.0.0/16
        bgp:
          asn_client: 65000
          asn_server: 7224
          hold: 30

        links:
          - name: mouse1
            ike:
              psk: REPLACEME
            ipsec:
              proto: esp
              dpd_interval: 10
              dpd_retry: 3
            esp:
              tcp_mss: 1387
              no_fragment: true
            tunnel:
              outside:
                client: "{{ cgw_external_ip }}"
                server: 52.44.66.123
              inside:
                client: 169.254.46.138/30
                server: 169.254.46.137/30
              mtu: 1436

          - name: mouse2
            ike:
              psk: REPLACEME
            ipsec:
              proto: esp
              dpd_interval: 10
              dpd_retry: 3
            esp:
              tcp_mss: 1387
              no_fragment: true
            tunnel:
              outside:
                client: "{{ cgw_external_ip }}"
                server: 52.45.104.171
              inside:
                client: 169.254.47.150/30
                server: 169.254.47.149/30
              mtu: 1436
  roles:
    - role: aws-cgateway-role
```

License
-------

MIT
