
#### Ensure the path to the custom Ansible module is exported
export ANSIBLE_LIBRARY="${pwd}/rhsso-automation-capabilities"


#### create python venv
```
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install ansible
pip install -r requirements-dev.txt
```

#### Configurations to run the Ansible playbook


#### The section to get the RHSSO URL and credentials set

```  
  vars:
    kc: 
      url: https://sso-dre002ng-dev.apps.sandbox.x8i5.p1.openshiftapps.com/
      user: admin
      pass: admin
```

#### Set the path to the Realm data on disk
```
  tasks:
    - name: Full path to data source
      set_fact:
        src_dp: "{{ playbook_dir }}/keycloak-fetch-bot/output/keycloak/ci0-realm/"
```

#### Run the playbook
```
ansible-playbok main.yaml

# local test server
docker run -it -p 8080:80 -p 8443:443 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin quay.io/keycloak/keycloak:9.0.3
ansible-playbok playbooks/test_import_realm.yml
```
