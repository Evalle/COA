<a name="top"></a>
### 1. Using Horizon, get the necessay credential variables to get access to Openstack environment through CLI.


    - Go to {project > Access and security > API access}, get the admin rc file admin-openrc.sh.
    - Copy/paste into the fiel in the CLI and source it
    - source demorc.sh

### 2. Create a project named "new-project1", a user "new-user1" with password "new-user1", and assign a new role role "manager" to that user.    

    openstack project create "project1"  
    openstack project list  
    openstack user create "new-user1"  
    openstack user list  
    openstack role create "manager"  
    openstack role list  

---

### 3. Without accessing Horizon, build a user credential file to get access to CLI openstack client.  

* project domain id :  default
* user domain id :  default
* project name : admin
* user name :  admin
* type of password :  password

If you are not sure about the argument grammar, issue the command **openstack --help**  

[https://github.com/AJNOURI/COA/blob/master/create-rc-file-without-horizon.md](https://github.com/AJNOURI/COA/blob/master/create-rc-file-without-horizon.md)


### 4. A new service is developped to manage projects IPv4 and IPv6 address space (IPAM).
You are asked to create a service and associate the appropriate endpoints:

Service
- type: ipam
- name: ipam
- description: ipam service

endpoint Interfaces
- public http://controller:60666
- admin http://controller:60666
- internal http://controller:60666

--

    openstack service create --name ipam --description "ipam service" ipam  
    openstack endpoint create --region RegionOne test public http://controller:60666  
    openstack endpoint create --region RegionOne test internal http://controller:60666  
    openstack endpoint create --region RegionOne test admin http://controller:60666  
    openstack endpoint list | grep test



---

Domain specific configurartion
------------


### 1. Create a domain named "domain1"
```
openstack domain create domain1 --description "domain 1"
openstack domain list
```

### 2. Create a project named "domain1_project1" within the domain "domain1"
```
openstack project create domain1_project1 --domain domain1
openstack project list
```

### 3. Create a user "domain1_admin" in the domain "domain1"
```
openstack user create domain1_admin --email "admin@domain1.com" --domain domain1 --password-prompt
openstack user list --domain domain1
```

### 4. Assign domain scope "admin" role to user "domain1_admin"
`openstack role add --user domain1_admin --domain domain1 admin`

### 5. Create a regular user within the domain "domain1"
```
openstack user create domain1_user1 --email "user1@domain1.com" --domain domain1 --password-prompt
openstack user list --domain domain1
```
