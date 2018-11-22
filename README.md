avinetworks.avimigrationtools
=========
Avi Ansible Role with utilities like traffic cutover

Requirements
------------
- `avisdk` python library is required and can be installed by:  
`pip install avisdk --upgrade`  

Role Dependencies
------------
- avinetworks.avisdk
  - To install these use the following command: `ansible-galaxy install -f avinetworks.avisdk`  


Role Variables
--------------

### avi_traffic Variables 

| Variable | Required | Default | Comments |
|----------|----------|---------|----------|
| `avi_vs_ip_address` | Yes | `None` | VIP of VS on which traffic needs to be sent |
| `avi_vs_type` | Yes | `None` | Type of VS |
| `avi_vs_port` | Yes | `None` | VS service port |
| `avi_vs_name` | Yes | `None` | Name of Avi VS |
| `avi_controller` | Yes | `None` | IP address of Avi controller |
| `avi_con_username` | Yes | `None` | Avi Controller username |
| `avi_con_password` | Yes | `None` | Avi controller password |
| `avi_con_tenant` | No | `admin` | Avi controller tenant name |


### netscaler_vs_status Variables 

| Variable | Required | Default | Comments |
|----------|----------|---------|----------|
| `ns_username` | Yes | `None` | Netscaler instance username |
| `ns_password` | Yes | `None` | Netscaler instance password|
| `vs_state` | Yes | `None` | State of VS to be changed |
| `vs_name` | Yes | `None` | Name of VS to be updated |
| `vs_type` | Yes | `None` | Type of vs options: lbsv,csvs|
| `ns_host` | Yes | `None` | Netscaler host IP |


Example Playbook
----------------

    - connection: local
      hosts: localhost
      roles:
      - avinetworks.avimigrationtools
      tasks:
      - avi_traffic:
          avi_con_password: '{{password}}'
          avi_con_tenant: admin
          avi_con_username: '{{username}}'
          avi_controller: '{{controller}}'
          avi_vs_ip_address: '{{ vs_ip }}'
          avi_vs_name: vs-1
          avi_vs_port: '80'
          avi_vs_type: http
        delay: 5
        name: 'Generate Avi virtualservice traffic: vs-1'
        register: result
        retries: 240
        tags:
        - vs-1
        - generate_traffic
        until: result.success == 0

License
-------
Apache 2.0

Author Information
------------------

Chaitanya Deshpande  
[Avi Networks](http://avinetworks.com)
