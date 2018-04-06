Apigee OPDK Setup Openldap
=========

This role sets up system packages that enable Openldap to be configured correctly for the Apigee platform. 

Requirements
------------

This role requires elevated system privilege. 

Role Variables
--------------
The `openldap` collection contains the names of the system packages that should be installed assuming it is available: 

    openldap:
    - openldap
    - openldap-clients
    - openldap-servers
    
Should the `openldap` packages fail to install this role will attempt to install version 2.4.0 specifically of openldap. 


    

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: apigee-opdk-setup-os-openldap }

License
-------

Apache 2.0

Author Information
------------------

Carlos Frias


<!-- BEGIN Google Required Disclaimer -->

# Not Google Product Clause

This is not an officially supported Google product.
<!-- END Google Required Disclaimer -->
<!-- BEGIN Google How To Contribute -->
# How to Contribute

We'd love to accept your patches and contributions to this project. Please review our [guidelines](CONTRIBUTION.md).
<!-- END Google How To Contribute -->
