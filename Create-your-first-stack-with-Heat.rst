####
Create your First Stack with Heat
####

===============================

In this guide, we will detail the steps of creating a stack via Heat.

We will write a template that describes our infrastructure and just deploy it with Heat! 

 
It's quick and easy ;)


.. contents::

Overview
========

The most important step in this guide is the creation of the Heat Orchestration Template or HOT !

A HOT template is defined in YAML or JSON. In this guide, we use YAML. we think it's clearer to read ;)

HOT is also composed of three main sections (see the figure below):

.. image:: https://raw.githubusercontent.com/MarouenMechtri/OpenStack-Heat-Installation/master/images/heat-template.jpg

Detailed information on parameters and resource types are available on these links: `HOT Specification <http://docs.openstack.org/developer/heat/template_guide/hot_spec.html>`_ and  `OpenStack Resource Types <http://docs.openstack.org/developer/heat/template_guide/openstack.html>`_


Using the above information, let's create a simple Hot to deploy one server ;)

* The template looks like this::

    heat_template_version: 2014-10-16

    description: A simple server.

    parameters:
      ImageID:
        type: string
        description: Image use to boot a server
      NetID:
        type: string
        description: Network ID for the server

    resources:
      server:
        type: OS::Nova::Server
        properties:
          image: { get_param: ImageID }
          flavor: m1.tiny
          networks:
            - network: { get_param: NetID }

    outputs:
      private_ip:
        description: IP address of the server in the private network
        value: { get_attr: [ server, first_address ] }

Create your stack
=================

Now the template is ready! let's create the stack ;)

* Create a simple credential file::

    vi demo_creds
    #Paste the following:
    export OS_TENANT_NAME=demo
    export OS_USERNAME=demo
    export OS_PASSWORD=demo_pass
    export OS_AUTH_URL=http://controller:5000/v2.0
    
* Create a stack from the template (file available `here <https://github.com/MarouenMechtri/Heat-Installation-for-OpenStack-Juno/blob/master/heat-templates/first-stack.yml>`_)::

    source demo_creds

    NET_ID=$(nova net-list | awk '/ demo-net / { print $2 }')

    heat stack-create -f first-stack.yml \
    -P "ImageID=cirros-0.3.3-x86_64;NetID=$NET_ID" First_Stack

    
Verify Stack creation
=====================

* Verify that the stack was created successfully::

    heat stack-list

Now you are finally done! You can enjoy your first stack ;)

Please contact us for any question or suggestion :)


License
=======

Institut Mines Télécom - Télécom SudParis  

Copyright (C) 2015  Authors

Original Authors -  Marouen Mechtri and  Chaima Ghribi 

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except 

in compliance with the License. You may obtain a copy of the License at::

    http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.


Contacts
========

Marouen Mechtri : marouen.mechtri@it-sudparis.eu

Chaima Ghribi: chaima.ghribi@it-sudparis.eu
