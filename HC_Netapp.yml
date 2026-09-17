---
- name: NetApp Cluster Comprehensive Health Check
  hosts: netapp
  gather_facts: false

  tasks:

    - name: Gather ONTAP information
      netapp.ontap.na_ontap_rest_info:
        hostname: "{{ ansible_host }}"
        username: "{{ ansible_user }}"
        password: "{{ ansible_password }}"
        validate_certs: false
        gather_subset:
          - cluster
          - cluster/nodes
          - storage/aggregates
      register: ontap

    - name: Cluster Summary
      debug:
        msg:
          - "Cluster: {{ ontap.ontap_info.cluster.name }}"
          - "Health: {{ ontap.ontap_info.cluster.health }}"

    - name: Node Health
      debug:
        msg: "Node {{ item.name }} Health={{ item.health }}"
      loop: "{{ ontap.ontap_info['cluster/nodes'].records }}"

    - name: Aggregate Status
      debug:
        msg: >
          Aggregate {{ item.name }}
          State={{ item.state }}
          Used={{ item.space.block_storage.used_percent | default('N/A') }}%
      loop: "{{ ontap.ontap_info['storage/aggregates'].records }}"
``
