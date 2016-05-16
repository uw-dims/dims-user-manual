.. _dimscli:

Developing modules for the DIMS CLI app (``dimscli``)
-----------------------------------------------------

.. _bootstrappingdimscli:

Bootstrapping the ``dimscli`` app for development
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. TODO(dittrich): Added on
.. todo::

    .. note::

        This section is an initial effort to show how to bootstrap development
        of the ``dimscli`` app.

        Sun Nov  8 15:43:26 PST 2015

    ..

..


#. Clone the repo ``python-dimscli`` from ``git.prisem.washington.edu``. This can be
   done by running ``dims.git.syncrepos``:

#. Prepare a new Python virtual environment with all of the DIMS pre-requisite
   tools necessary for DIMS software development:

   .. code-block:: none

       [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $ VENV=dimscli dimsenv.install.user
       sudo password:

       PLAY [Install python virtual environment] *************************************

       ...

       PLAY RECAP ********************************************************************
       localhost                  : ok=30   changed=19   unreachable=0    failed=0

   ..

   The new ``dimscli`` virtual environment should show up as an option for
   ``workon``:

   .. code-block:: none

       [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $ workon
       dimscli
       dimsenv

   ..

#. Invoke the new ``dimscli`` Python virtual environment.

   .. code-block:: none

       [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $ workon dimscli
       [+++] Virtual environment 'dimscli' activated [ansible-playbooks v1.2.113]

   ..

#. Because this is a new Python virtual environment created with the DIMS
   build tools, it only has those Python packages defined in Ansible
   playbooks role ``python-virtualenv``.

   .. todo::

      Add an intersphinx link to the details about this Ansible role.

   ..

   The first time you try to run ``dimscli``, or any time that you change
   any of the pre-requisites used for programming ``dimscli`` modules,
   you must use ``pip`` to update and/or install the required
   packages. These will eventually be added to the defaults for the
   ``dimsenv`` standard virtual environment.

   .. code-block:: none

       [dimscli] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $ pip install -U -r requirements.txt
       Collecting pbr<2.0,>=1.4 (from -r requirements.txt (line 1))
       Using cached pbr-1.8.1-py2.py3-none-any.whl
       Collecting six>=1.9.0 (from -r requirements.txt (line 2))
       Using cached six-1.10.0-py2.py3-none-any.whl
       Requirement already up-to-date: Babel>=1.3 in /home/dittrich/dims/envs/dimscli/lib/python2.7/site-packages (from -r requirements.txt (line 3))
       Collecting cliff>=1.14.0 (from -r requirements.txt (line 4))
       Downloading cliff-1.15.0-py2-none-any.whl
       Collecting keystoneauth1>=1.0.0 (from -r requirements.txt (line 5))
       Downloading keystoneauth1-1.2.0-py2.py3-none-any.whl (149kB)
       100% |████████████████████████████████| 151kB 2.7MB/s
       Collecting os-client-config!=1.6.2,>=1.4.0 (from -r requirements.txt (line 6))
       Downloading os_client_config-1.10.1-py2.py3-none-any.whl (42kB)
       100% |████████████████████████████████| 45kB 6.0MB/s

       ...

       Running setup.py bdist_wheel for msgpack-python
       Stored in directory: /home/dittrich/.cache/pip/wheels/f3/97/a5/dd6e3b680de10b689464c44bc211239d1fe54bd296ff860897
       Running setup.py bdist_wheel for functools32
       Stored in directory: /home/dittrich/.cache/pip/wheels/38/c6/c7/ee17acd621120c302e25c2fa8b3a8b235d5d1137c6ab4c9728
       Successfully built simplejson warlock msgpack-python functools32
       Installing collected packages: msgpack-python, oslo.serialization, python-keystoneclient, simplejson,
       python-neutronclient, functools32, jsonschema, jsonpointer, jsonpatch, warlock, python-glanceclient,
       python-novaclient, python-cinderclient, python-openstackclient

       Successfully installed functools32-3.2.3.post2 jsonpatch-1.12 jsonpointer-1.10 jsonschema-2.5.1 msgpack-python-0.4.6
       oslo.serialization-1.11.0 python-cinderclient-1.4.0 python-glanceclient-1.1.0 python-keystoneclient-1.8.1
       python-neutronclient-3.1.0 python-novaclient-2.34.0 python-openstackclient-1.8.0 simplejson-3.8.1 warlock-1.2.0
       PrettyTable-0.7.2 appdirs-1.4.0 cliff-1.15.0 cliff-tablib-1.1 cmd2-0.6.8 debtcollector-0.10.0 iso8601-0.1.11
       keystoneauth1-1.2.0 monotonic-0.4 netaddr-0.7.18 netifaces-0.10.4 os-client-config-1.10.1 oslo.config-2.6.0
       oslo.i18n-2.7.0 oslo.utils-2.7.0 oslosphinx-3.3.1 pbr-1.8.1 pyparsing-2.0.5 pytz-2015.7 requests-2.8.1
       six-1.10.0 stevedore-1.9.0 tablib-0.10.0 unicodecsv-0.14.1 wrapt-1.10.5

   ..

#. Once all the pre-requisite packages are installed in the virtual environment,
   install the ``dimscli`` app and its modules as well using ``python setup.py
   install`` or ``pip install -e .`` (either will work):

   .. code-block:: none

       [dimscli] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $ python setup.py install
       running install
       [pbr] Writing ChangeLog
       [pbr] Generating ChangeLog
       [pbr] ChangeLog complete (0.0s)
       [pbr] Generating AUTHORS
       [pbr] AUTHORS complete (0.0s)
       running build
       running build_py
       creating build
       creating build/lib
       creating build/lib/dimscli
       creating build/lib/dimscli/common

       ...

       byte-compiling /home/dittrich/dims/envs/dimscli/lib/python2.7/site-packages/dimscli/common/timing.py to timing.pyc
       byte-compiling /home/dittrich/dims/envs/dimscli/lib/python2.7/site-packages/dimscli/common/context.py to context.pyc
       byte-compiling /home/dittrich/dims/envs/dimscli/lib/python2.7/site-packages/dimscli/common/clientmanager.py to clientmanager.pyc
       byte-compiling /home/dittrich/dims/envs/dimscli/lib/python2.7/site-packages/dimscli/common/logs.py to logs.pyc
       byte-compiling /home/dittrich/dims/envs/dimscli/lib/python2.7/site-packages/dimscli/common/utils.py to utils.pyc
       running install_egg_info
       Copying python_dimscli.egg-info to /home/dittrich/dims/envs/dimscli/lib/python2.7/site-packages/python_dimscli-0.0.1.dev391-py2.7.egg-info
       running install_scripts
       Installing dimscli script to /home/dittrich/dims/envs/dimscli/bin

   ..

#. Run the ``dimscli`` app like any other program, directly from the command line.

   There are two ways to use ``dimscli``.

   * As a single command with command line options like other Linux commands


     .. code-block:: none

         [dimscli] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $ dimscli --version
         dimscli 0.0.1
         [dimscli] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $

     ..

   * As an interactive shell that allows you to run multiple commands in
     sequence within the same context (i.e., the same state, or runtime settings
     you invoke while in the shell) by just just the program name and no
     arguments or options.

     .. code-block:: none

          [dimscli] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $ dimscli
          defaults: {u'auth_type': 'password', u'compute_api_version': u'2', 'key': None, u'database_api_version': u'1.0',
          'api_timeout': None, u'baremetal_api_version': u'1', 'cacert': None, u'image_api_use_tasks': False,
          u'floating_ip_source': u'neutron', u'orchestration_api_version': u'1', u'interface': None, u'network_api_version':
          u'2.0', u'image_format': u'qcow2', u'object_api_version': u'1', u'image_api_version': u'2', 'verify': True,
          u'identity_api_version': u'2.0', u'volume_api_version': u'1', 'cert': None, u'secgroup_source': u'neutron',
          u'dns_api_version': u'2', u'disable_vendor_agent': {}}
          cloud cfg: {'auth_type': 'password', u'compute_api_version': u'2', u'orchestration_api_version': u'1',
          u'database_api_version': u'1.0', 'cacert': None, u'network_api_version': u'2.0', u'image_format': u'qcow2',
          u'object_api_version': u'1', u'image_api_version': u'2', 'verify': True, u'dns_api_version': u'2',
          'verbose_level': '1', 'region_name': '', 'api_timeout': None, u'baremetal_api_version': u'1', 'auth': {},
          'default_domain': 'default', u'image_api_use_tasks': False, u'floating_ip_source': u'neutron', 'key': None,
          'timing': False, 'deferred_help': False, u'identity_api_version': u'2.0', u'volume_api_version': u'1',
          'cert': None, u'secgroup_source': u'neutron', u'interface': None, u'disable_vendor_agent': {}}
          compute API version 2, cmd group dims.compute.v2
          network version 2.0 is not in supported versions 2
          network API version 2.0, cmd group dims.network.v2
          image API version 2, cmd group dims.image.v2
          volume API version 1, cmd group dims.volume.v1
          identity API version 2.0, cmd group dims.identity.v2
          object_store API version 1, cmd group dims.object_store.v1
          (dimscli) help

          Shell commands (type help <topic>):
          ===================================
          cmdenvironment  edit  hi       l   list  pause  r    save  shell      show
          ed              help  history  li  load  py     run  set   shortcuts

          Undocumented commands:
          ======================
          EOF  eof  exit  q  quit

          Application commands (type help <topic>):
          =========================================
          aggregate add host     host show              role list
          aggregate create       ip fixed add           role remove
          aggregate delete       ip fixed remove        role show
          aggregate list         ip floating add        security group create
          aggregate remove host  ip floating create     security group delete
          aggregate set          ip floating delete     security group list
          aggregate show         ip floating list       security group rule create
          catalog list           ip floating pool list  security group rule delete
          catalog show           ip floating remove     security group rule list
          command list           keypair create         security group set
          complete               keypair delete         security group show
          configuration show     keypair list           server create
          console log show       keypair show           server delete
          console url show       module list            server image create
          container create       network create         server list
          container delete       network delete         server reboot
          container list         network list           server rebuild
          container save         network set            server set
          container show         network show           server show
          endpoint create        object create          server ssh
          endpoint delete        object delete          service create
          endpoint list          object list            service delete
          endpoint show          object save            service list
          extension list         object show            service show
          flavor create          project create         token issue
          flavor delete          project delete         token revoke
          flavor list            project list           user create
          flavor set             project set            user delete
          flavor show            project show           user list
          flavor unset           role add               user role list
          help                   role create            user set
          host list              role delete            user show

          (dimscli) exit
          END return value: 0
          [dimscli] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $

     ..

.. _commandStructure:

Command Structure
~~~~~~~~~~~~~~~~~

The ``dimscli`` shell follows the ``openstack`` client in the manner in which
commands are to be constructed. See the Openstack `Command Structure`_ page
for details. To quote:

.. epigraph::

    *Commands consist of an object described by one or more words followed by an
    action. Commands that require two objects have the primary object ahead of
    the action and the secondary object after the action. Any positional
    arguments identifying the objects shall appear in the same order as the
    objects. In badly formed English it is expressed as “(Take) object1 (and
    perform) action (using) object2 (to it)."*

    .. code-block:: none

        <object-1> <action> <object-2>

    ..

    *Examples:*

    .. code-block:: none

        $ group add user <group> <user>

        $ volume type list   # 'volume type' is a two-word single object

    ..

..

.. _Command Structure: http://docs.openstack.org/developer/python-openstackclient/commands.html


.. _completingdimscli:

Completing commands in ``dimscli``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The initial implementation of ``dimscli`` ported from the ``openstacklient``
code base does not have much actual code underlying it, though the
scaffolding of ``openstacklient`` and many of its defined modules are
currently configured in the code. You can see the modules that are
not there by simply asking for ``dimscli --help`` and noting the
errors (and what they point to, which indicates which code you
need to seek out to use and/or replace.)

.. code-block:: none

    [dimscli] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $ dimscli --help
    defaults: {u'auth_type': 'password', u'compute_api_version': u'2', 'key': None, u'database_api_version': u'1.0', 'api_timeout': None, u'baremetal_api_version': u'1', 'cacert': None, u'image_api_use_tasks
    ': False, u'floating_ip_source': u'neutron', u'orchestration_api_version': u'1', u'interface': None, u'network_api_version': u'2.0', u'image_format': u'qcow2', u'object_api_version': u'1', u'image_api_ve
    rsion': u'2', 'verify': True, u'identity_api_version': u'2.0', u'volume_api_version': u'1', 'cert': None, u'secgroup_source': u'neutron', u'dns_api_version': u'2', u'disable_vendor_agent': {}}
    cloud cfg: {'auth_type': 'password', u'compute_api_version': u'2', u'orchestration_api_version': u'1', u'database_api_version': u'1.0', 'cacert': None, u'network_api_version': u'2.0', u'image_format': u'
    qcow2', u'object_api_version': u'1', u'image_api_version': u'2', 'verify': True, u'dns_api_version': u'2', 'verbose_level': '1', 'region_name': '', 'api_timeout': None, u'baremetal_api_version': u'1', 'a
    uth': {}, 'default_domain': 'default', u'image_api_use_tasks': False, u'floating_ip_source': u'neutron', 'key': None, 'timing': False, 'deferred_help': True, u'identity_api_version': u'2.0', u'volume_api
    _version': u'1', 'cert': None, u'secgroup_source': u'neutron', u'interface': None, u'disable_vendor_agent': {}}
    compute API version 2, cmd group dims.compute.v2
    network version 2.0 is not in supported versions 2
    network API version 2.0, cmd group dims.network.v2
    image API version 2, cmd group dims.image.v2
    volume API version 1, cmd group dims.volume.v1
    identity API version 2.0, cmd group dims.identity.v2
    object_store API version 1, cmd group dims.object_store.v1
    usage: dimscli [--version] [-v] [--log-file LOG_FILE] [-q] [-h] [--debug]
                   [--os-cloud <cloud-config-name>]
                   [--os-region-name <auth-region-name>]
                   [--os-cacert <ca-bundle-file>] [--verify | --insecure]
                   [--os-default-domain <auth-domain>]
     ...

      --os-object-api-version <object-api-version>
                            Object API version, default=1 (Env:
                            OS_OBJECT_API_VERSION)

    Commands:
    Could not load EntryPoint.parse('aggregate_add_host = dimscli.compute.v2.aggregate:AddAggregateHost')
    Could not load EntryPoint.parse('aggregate_create = dimscli.compute.v2.aggregate:CreateAggregate')
    Could not load EntryPoint.parse('aggregate_delete = dimscli.compute.v2.aggregate:DeleteAggregate')
    Could not load EntryPoint.parse('aggregate_list = dimscli.compute.v2.aggregate:ListAggregate')
    Could not load EntryPoint.parse('aggregate_remove_host = dimscli.compute.v2.aggregate:RemoveAggregateHost')
    Could not load EntryPoint.parse('aggregate_set = dimscli.compute.v2.aggregate:SetAggregate')
    Could not load EntryPoint.parse('aggregate_show = dimscli.compute.v2.aggregate:ShowAggregate')
    Could not load EntryPoint.parse('catalog_list = dimscli.identity.v2_0.catalog:ListCatalog')
    Could not load EntryPoint.parse('catalog_show = dimscli.identity.v2_0.catalog:ShowCatalog')
    Could not load EntryPoint.parse('command_list = dimscli.common.module:ListCommand')
      complete       print bash completion command
    Could not load EntryPoint.parse('configuration_show = dimscli.common.configuration:ShowConfiguration')
    Could not load EntryPoint.parse('console_log_show = dimscli.compute.v2.console:ShowConsoleLog')
    Could not load EntryPoint.parse('console_url_show = dimscli.compute.v2.console:ShowConsoleURL')
    Could not load EntryPoint.parse('container_create = dimscli.object.v1.container:CreateContainer')
    Could not load EntryPoint.parse('container_delete = dimscli.object.v1.container:DeleteContainer')
    Could not load EntryPoint.parse('container_list = dimscli.object.v1.container:ListContainer')
    Could not load EntryPoint.parse('container_save = dimscli.object.v1.container:SaveContainer')
    Could not load EntryPoint.parse('container_show = dimscli.object.v1.container:ShowContainer')
    Could not load EntryPoint.parse('endpoint_create = dimscli.identity.v2_0.endpoint:CreateEndpoint')
    Could not load EntryPoint.parse('endpoint_delete = dimscli.identity.v2_0.endpoint:DeleteEndpoint')
    Could not load EntryPoint.parse('endpoint_list = dimscli.identity.v2_0.endpoint:ListEndpoint')
    Could not load EntryPoint.parse('endpoint_show = dimscli.identity.v2_0.endpoint:ShowEndpoint')
    Could not load EntryPoint.parse('extension_list = dimscli.common.extension:ListExtension')
    Could not load EntryPoint.parse('flavor_create = dimscli.compute.v2.flavor:CreateFlavor')
    Could not load EntryPoint.parse('flavor_delete = dimscli.compute.v2.flavor:DeleteFlavor')
    Could not load EntryPoint.parse('flavor_list = dimscli.compute.v2.flavor:ListFlavor')
    Could not load EntryPoint.parse('flavor_set = dimscli.compute.v2.flavor:SetFlavor')
    Could not load EntryPoint.parse('flavor_show = dimscli.compute.v2.flavor:ShowFlavor')
    Could not load EntryPoint.parse('flavor_unset = dimscli.compute.v2.flavor:UnsetFlavor')
      help           print detailed help for another command
    Could not load EntryPoint.parse('host_list = dimscli.compute.v2.host:ListHost')
    Could not load EntryPoint.parse('host_show = dimscli.compute.v2.host:ShowHost')
    Could not load EntryPoint.parse('ip_fixed_add = dimscli.compute.v2.fixedip:AddFixedIP')
    Could not load EntryPoint.parse('ip_fixed_remove = dimscli.compute.v2.fixedip:RemoveFixedIP')
    Could not load EntryPoint.parse('ip_floating_add = dimscli.compute.v2.floatingip:AddFloatingIP')
    Could not load EntryPoint.parse('ip_floating_create = dimscli.compute.v2.floatingip:CreateFloatingIP')
    Could not load EntryPoint.parse('ip_floating_delete = dimscli.compute.v2.floatingip:DeleteFloatingIP')
    Could not load EntryPoint.parse('ip_floating_list = dimscli.compute.v2.floatingip:ListFloatingIP')
    Could not load EntryPoint.parse('ip_floating_pool_list = dimscli.compute.v2.floatingippool:ListFloatingIPPool')
    Could not load EntryPoint.parse('ip_floating_remove = dimscli.compute.v2.floatingip:RemoveFloatingIP')
    Could not load EntryPoint.parse('keypair_create = dimscli.compute.v2.keypair:CreateKeypair')
    Could not load EntryPoint.parse('keypair_delete = dimscli.compute.v2.keypair:DeleteKeypair')
    Could not load EntryPoint.parse('keypair_list = dimscli.compute.v2.keypair:ListKeypair')
    Could not load EntryPoint.parse('keypair_show = dimscli.compute.v2.keypair:ShowKeypair')
    Could not load EntryPoint.parse('module_list = dimscli.common.module:ListModule')
    Could not load EntryPoint.parse('network_create = dimscli.network.v2.network:CreateNetwork')
    Could not load EntryPoint.parse('network_delete = dimscli.network.v2.network:DeleteNetwork')
    Could not load EntryPoint.parse('network_list = dimscli.network.v2.network:ListNetwork')
    Could not load EntryPoint.parse('network_set = dimscli.network.v2.network:SetNetwork')
    Could not load EntryPoint.parse('network_show = dimscli.network.v2.network:ShowNetwork')
    Could not load EntryPoint.parse('object_create = dimscli.object.v1.object:CreateObject')
    Could not load EntryPoint.parse('object_delete = dimscli.object.v1.object:DeleteObject')
    Could not load EntryPoint.parse('object_list = dimscli.object.v1.object:ListObject')
    Could not load EntryPoint.parse('object_save = dimscli.object.v1.object:SaveObject')
    Could not load EntryPoint.parse('object_show = dimscli.object.v1.object:ShowObject')
    Could not load EntryPoint.parse('project_create = dimscli.identity.v2_0.project:CreateProject')
    Could not load EntryPoint.parse('project_delete = dimscli.identity.v2_0.project:DeleteProject')
    Could not load EntryPoint.parse('project_list = dimscli.identity.v2_0.project:ListProject')
    Could not load EntryPoint.parse('project_set = dimscli.identity.v2_0.project:SetProject')
    Could not load EntryPoint.parse('project_show = dimscli.identity.v2_0.project:ShowProject')
    Could not load EntryPoint.parse('role_add = dimscli.identity.v2_0.role:AddRole')
    Could not load EntryPoint.parse('role_create = dimscli.identity.v2_0.role:CreateRole')
    Could not load EntryPoint.parse('role_delete = dimscli.identity.v2_0.role:DeleteRole')
    Could not load EntryPoint.parse('role_list = dimscli.identity.v2_0.role:ListRole')
    Could not load EntryPoint.parse('role_remove = dimscli.identity.v2_0.role:RemoveRole')
    Could not load EntryPoint.parse('role_show = dimscli.identity.v2_0.role:ShowRole')
    Could not load EntryPoint.parse('security_group_create = dimscli.compute.v2.security_group:CreateSecurityGroup')
    Could not load EntryPoint.parse('security_group_delete = dimscli.compute.v2.security_group:DeleteSecurityGroup')
    Could not load EntryPoint.parse('security_group_list = dimscli.compute.v2.security_group:ListSecurityGroup')
    Could not load EntryPoint.parse('security_group_rule_create = dimscli.compute.v2.security_group:CreateSecurityGroupRule')
    Could not load EntryPoint.parse('security_group_rule_delete = dimscli.compute.v2.security_group:DeleteSecurityGroupRule')
    Could not load EntryPoint.parse('security_group_rule_list = dimscli.compute.v2.security_group:ListSecurityGroupRule')
    Could not load EntryPoint.parse('security_group_set = dimscli.compute.v2.security_group:SetSecurityGroup')
    Could not load EntryPoint.parse('security_group_show = dimscli.compute.v2.security_group:ShowSecurityGroup')
    Could not load EntryPoint.parse('server_create = dimscli.compute.v2.server:CreateServer')
    Could not load EntryPoint.parse('server_delete = dimscli.compute.v2.server:DeleteServer')
    Could not load EntryPoint.parse('server_image_create = dimscli.compute.v2.server:CreateServerImage')
    Could not load EntryPoint.parse('server_list = dimscli.compute.v2.server:ListServer')
    Could not load EntryPoint.parse('server_reboot = dimscli.compute.v2.server:RebootServer')
    Could not load EntryPoint.parse('server_rebuild = dimscli.compute.v2.server:RebuildServer')
    Could not load EntryPoint.parse('server_set = dimscli.compute.v2.server:SetServer')
    Could not load EntryPoint.parse('server_show = dimscli.compute.v2.server:ShowServer')
    Could not load EntryPoint.parse('server_ssh = dimscli.compute.v2.server:SshServer')
    Could not load EntryPoint.parse('service_create = dimscli.identity.v2_0.service:CreateService')
    Could not load EntryPoint.parse('service_delete = dimscli.identity.v2_0.service:DeleteService')
    Could not load EntryPoint.parse('service_list = dimscli.identity.v2_0.service:ListService')
    Could not load EntryPoint.parse('service_show = dimscli.identity.v2_0.service:ShowService')
    Could not load EntryPoint.parse('token_issue = dimscli.identity.v2_0.token:IssueToken')
    Could not load EntryPoint.parse('token_revoke = dimscli.identity.v2_0.token:RevokeToken')
    Could not load EntryPoint.parse('user_create = dimscli.identity.v2_0.user:CreateUser')
    Could not load EntryPoint.parse('user_delete = dimscli.identity.v2_0.user:DeleteUser')
    Could not load EntryPoint.parse('user_list = dimscli.identity.v2_0.user:ListUser')
    Could not load EntryPoint.parse('user_role_list = dimscli.identity.v2_0.role:ListUserRole')
    Could not load EntryPoint.parse('user_set = dimscli.identity.v2_0.user:SetUser')
    Could not load EntryPoint.parse('user_show = dimscli.identity.v2_0.user:ShowUser')
    END return value: 1
    [dimscli] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop) $

..

Using the last error message above as an example, there needs to be a module
named ``$GIT/python-dimscli/dimscli/identity/v2_0/user.py`` with a
class ``ShowUser``. Look in the ``python-openstack/openstack/identity/v2_0/``
directory for their ``user.py`` and build off that example.

.. attention::

    Clone the ``python-openstackclient`` repo using ``git clone
    https://git.openstack.org/openstack/python-openstackclient`` and
    see the ``cliff`` documentation, Section `Exploring the Demo App`_, for how
    this works.

..

.. _Exploring the Demo App: http://docs.openstack.org/developer/cliff/demoapp.html

.. attention::

    See the file ``$GIT/python-dimscli/README.rst`` for more
    documentation produced during initial creation of the openstackclient
    fork of ``dimscli``.

    .. todo::

        Make this an intersphinx link as soon as documentation auto-generation
        is working for that repo.

    ..

..

``cliff`` supports list formatting in tables, CSV, JSON, etc., but not in shell
format. That is only supported by the ``ShowOne`` class, which is not what we
want for producing a set of variables for insertion into shell environments.

.. code-block:: none

    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ dimscli list nodes
    +----------------+---------------+
    | Node           | Address       |
    +----------------+---------------+
    | b52            | 10.86.86.7    |
    | consul-breathe | 10.142.29.117 |
    | consul-echoes  | 10.142.29.116 |
    | consul-seamus  | 10.142.29.120 |
    | dimsdemo1      | 10.86.86.2    |
    | dimsdev1       | 10.86.86.5    |
    | dimsdev2       | 10.86.86.5    |
    | four           | 192.168.0.101 |
    +----------------+---------------+

..

.. code-block:: none

    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ dimscli list nodes -f csv
    "Node","Address"
    "b52","10.86.86.7"
    "consul-breathe","10.142.29.117"
    "consul-echoes","10.142.29.116"
    "consul-seamus","10.142.29.120"
    "dimsdemo1","10.86.86.2"
    "dimsdev1","10.86.86.5"
    "dimsdev2","10.86.86.5"
    "four","192.168.0.101"

..


.. code-block:: none

    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ dimscli list nodes -f json
    [{"Node": "b52", "Address": "10.86.86.7"}, {"Node": "consul-breathe", "Address": "10.142.29.117"}, {"Node": "consul-echoes", "Address": "10.142.29.116"}, {"Node": "consul-seamus", "Address": "10.142.29.120"}, {"Node": "dimsdemo1", "Address": "10.86.86.2"}, {"Node": "dimsdev1", "Address": "10.86.86.5"}, {"Node": "dimsdev2", "Address": "10.86.86.5"}, {"Node": "four", "Address": "192.168.0.101"}]
    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ dimscli list nodes -f json | python -m json.tool
    [
        {
            "Address": "10.86.86.7",
            "Node": "b52"
        },
        {
            "Address": "10.142.29.117",
            "Node": "consul-breathe"
        },
        {
            "Address": "10.142.29.116",
            "Node": "consul-echoes"
        },
        {
            "Address": "10.142.29.120",
            "Node": "consul-seamus"
        },
        {
            "Address": "10.86.86.2",
            "Node": "dimsdemo1"
        },
        {
            "Address": "10.86.86.5",
            "Node": "dimsdev1"
        },
        {
            "Address": "10.86.86.5",
            "Node": "dimsdev2"
        },
        {
            "Address": "192.168.0.101",
            "Node": "four"
        }
    ]

..


To produce the list in the form of shell variables, we need to create a custom
formatter and load it into the ``dimscli`` shell via Stevedore.

.. TODO(dittrich): Integrate a description of Git commit 30d096e4f2b25a37fb5eae4dfe9420b0e3960757

.. todo::

    Integrate a description of this Git commit:

    .. code-block:: diff

        commit 30d096e4f2b25a37fb5eae4dfe9420b0e3960757
        Author: Dave Dittrich <dittrich@u.washington.edu>
        Date:   Wed Dec 30 23:25:19 2015 -0800

            Add initial lister with custom shell formatter for listing Consul nodes

        diff --git a/dimscli/formatters/shell.py b/dimscli/formatters/shell.py
        new file mode 100644
        index 0000000..a9c86d7
        --- /dev/null
        +++ b/dimscli/formatters/shell.py
        @@ -0,0 +1,50 @@
        +"""Output formatters using shell syntax.
        +"""
        +
        +from cliff.formatters.base import ListFormatter, SingleFormatter
        +
        +import argparse
        +import six
        +
        +
        +class DIMSShellFormatter(ListFormatter, SingleFormatter):
        +
        +    def add_argument_group(self, parser):
        +        group = parser.add_argument_group(
        +            title='shell formatter',
        +            description='a format a UNIX shell can parse (variable="value")',
        +        )
        +        group.add_argument(
        +            '--variable',
        +            action='append',
        +            default=[],
        +            dest='variables',
        +            metavar='VARIABLE',
        +            help=argparse.SUPPRESS,
        +        )
        +        group.add_argument(
        +            '--prefix',
        +            action='store',
        +            default='',
        +            dest='prefix',
        +            help='add a prefix to all variable names',
        +        )
        +
        +    def emit_one(self, column_names, data, stdout, parsed_args):
        +        variable_names = [c.lower().replace(' ', '_')
        +                          for c in column_names
        +                          ]
        +        desired_columns = parsed_args.variables
        +        for name, value in zip(variable_names, data):
        +            if name in desired_columns or not desired_columns:
        +                if isinstance(value, six.string_types):
        +                    value = value.replace('"', '\\"')
        +                stdout.write('%s%s="%s"\n' % (parsed_args.prefix, name, value))
        +        return
        +
        +    def emit_list(self, column_names, data, stdout, parsed_args):
        +        for name, value in data:
        +            if isinstance(value, six.string_types):
        +                value = value.replace('"', '\\"')
        +            stdout.write('%s%s="%s"\n' % (parsed_args.prefix, name, value))
        +        return
        \ No newline at end of file
        diff --git a/dimscli/list.py b/dimscli/list.py
        index 3dcee4a..45acdda 100644
        --- a/dimscli/list.py
        +++ b/dimscli/list.py
        @@ -1,8 +1,10 @@
         import logging
         import os
        +import consulate
        +import json

         from cliff.lister import Lister
        -
        +from cliff.show import ShowOne

         class Files(Lister):
             """Show a list of files in the current directory.
        @@ -16,3 +18,17 @@ class Files(Lister):
                 return (('Name', 'Size'),
                         ((n, os.stat(n).st_size) for n in os.listdir('.'))
                         )
        +
        +class Nodes(Lister):
        +    """Show a list of nodes registered in Consul.
        +
        +    """
        +
        +    log = logging.getLogger(__name__)
        +
        +    def take_action(self, parsed_args):
        +        consul = consulate.Consul()
        +        nodes = consul.catalog.nodes()
        +        columns = ('Node', 'Address')
        +        data = ((node['Node'], node['Address']) for node in nodes)
        +        return (columns, data)
        diff --git a/requirements.txt b/requirements.txt
        index b74529f..7c54111 100644
        --- a/requirements.txt
        +++ b/requirements.txt
        @@ -22,3 +22,4 @@ stevedore>=1.5.0 # Apache-2.0
         unicodecsv>=0.8.0
         PyYAML>=3.1.0
         python-openstackclient>=1.8.0
        +consulate==0.6.0
        diff --git a/setup.cfg b/setup.cfg
        index f869411..2f17c6c 100644
        --- a/setup.cfg
        +++ b/setup.cfg
        @@ -35,9 +35,16 @@ dims.cli =
             command_list = dimscli.common.module:ListCommand
             module_list = dimscli.common.module:ListModule
             list_files = dimscli.list:Files
        +    list_nodes = dimscli.list:Nodes
             files = dimscli.list:Files
             show_file = dimscli.show:File

        +cliff.formatter.list =
        +    shell = dimscli.formatters.shell:DIMSShellFormatter
        +
        +cliff.formatter.show =
        +    shell = dimscli.formatters.shell:DIMSShellFormatter
        +
         dims.cli.base =
             compute = dimscli.compute.client
         #   identity = dimscli.identity.client

    ..

..

After adding the new formatter, it is possible to extract the list of nodes registered
with Consul and produce a set of variable declarations from the list.

.. code-block:: none

    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ dimscli list nodes -f shell
    b52="10.86.86.7"
    consul_breathe="10.142.29.117"
    consul_echoes="10.142.29.116"
    consul_seamus="10.142.29.120"
    dimsdemo1="10.86.86.2"
    dimsdev1="10.86.86.5"
    dimsdev2="10.86.86.5"
    four="192.168.0.101"

..

In practice, you may wish to insert these as variables in the shell's ``set`` using
the ``eval`` statement for use when invoking shell commands:

.. code-block:: none

    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ eval $(dimscli list nodes -f shell --prefix=DIMS_)
    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ set | grep DIMS_
    DIMS_REV=unspecified
    DIMS_VERSION='1.6.124 (dims-ci-utils)'
    DIMS_b52=10.86.86.7
    DIMS_consul_breathe=10.142.29.117
    DIMS_consul_echoes=10.142.29.116
    DIMS_consul_seamus=10.142.29.120
    DIMS_dimsdemo1=10.86.86.2
    DIMS_dimsdev2=10.86.86.5
    DIMS_four=192.168.0.101
        echo "REV:     $DIMS_REV";
        echo "[dims-ci-utils version $(version) (rev $DIMS_REV)]";
            echo "$PROGRAM $DIMS_VERSION";
            echo "$BASE $DIMS_VERSION";

..

.. _addingnewcolumns:

Adding New Columns to Output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Say we want to also include the Consul status, to help determine which node is
currently the *Leader* in a cluster, which are a *Peer* in the cluster, and which
are simply an *Agent* that is proxying to the cluster.

The changes to existing code to affect this new feature are shown here:

.. code-block:: diff

    commit caab2d05274898878e1123bd337b431c8d2f2a8e
    Author: Dave Dittrich <dittrich@u.washington.edu>
    Date:   Sat Jan 2 12:53:56 2016 -0800

        Add Consul node status to 'nodes list' output

    diff --git a/dimscli/list.py b/dimscli/list.py
    index 45acdda..3893b10 100644
    --- a/dimscli/list.py
    +++ b/dimscli/list.py
    @@ -26,9 +26,35 @@ class Nodes(Lister):

         log = logging.getLogger(__name__)

    +    def get_node_status(self):
    +        """
    +        Determine the status from Consul
    +
    +        :return: None
    +        """
    +        self.leaderDict = dict(zip(['Address', 'Port'],
    +                                   self.consul.status.leader().split(":")))
    +        self.peersDictList = [dict(zip(['Address', 'Port'], p.split(":")))
    +                              for p in self.consul.status.peers()]
    +
    +    def status(self, address):
    +        """
    +        Determine node status as returned from Consul.
    +
    +        :param address: IP address to check
    +        :return: One of: "Leader", "Peer", or "Agent"
    +        """
    +        if address in self.leaderDict.values():
    +            return "Leader"
    +        elif address in [p['Address'] for p in self.peersDictList]:
    +            return "Peer"
    +        else:
    +            return "Agent"
    +
         def take_action(self, parsed_args):
    -        consul = consulate.Consul()
    -        nodes = consul.catalog.nodes()
    -        columns = ('Node', 'Address')
    -        data = ((node['Node'], node['Address']) for node in nodes)
    +        self.consul = consulate.Consul()
    +        nodes = self.consul.catalog.nodes()
    +        self.get_node_status()
    +        columns = ('Node', 'Address', 'Status')
    +        data = ((node['Node'], node['Address'], self.status(node['Address'])) for node in nodes)
             return (columns, data)

..

.. code-block:: none

    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ dimscli nodes list
    +-----------+---------------+--------+
    | Node      | Address       | Status |
    +-----------+---------------+--------+
    | b52       | 10.86.86.2    | Agent  |
    | breathe   | 10.142.29.117 | Leader |
    | dimsdemo1 | 10.86.86.3    | Agent  |
    | echoes    | 10.142.29.116 | Peer   |
    | seamus    | 10.142.29.120 | Peer   |
    +-----------+---------------+--------+
    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ dimscli nodes list -f csv
    "Node","Address","Status"
    "b52","10.86.86.2","Agent"
    "breathe","10.142.29.117","Leader"
    "dimsdemo1","10.86.86.3","Agent"
    "echoes","10.142.29.116","Peer"
    "seamus","10.142.29.120","Peer"
    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ dimscli nodes list -f json | python -mjson.tool
    [
        {
            "Address": "10.86.86.2",
            "Node": "b52",
            "Status": "Agent"
        },
        {
            "Address": "10.142.29.117",
            "Node": "breathe",
            "Status": "Leader"
        },
        {
            "Address": "10.86.86.3",
            "Node": "dimsdemo1",
            "Status": "Agent"
        },
        {
            "Address": "10.142.29.116",
            "Node": "echoes",
            "Status": "Peer"
        },
        {
            "Address": "10.142.29.120",
            "Node": "seamus",
            "Status": "Peer"
        }
    ]

..

If we wish to turn a subset of this table into variables, using the ``shell`` output
feature added above, we need to select a pair of columns (to map to *Variable=Value*
in the output). The results could then be used in Ansible playbooks, shell scripts,
selecting color for nodes in a graph, or any number of other purposes.

.. code-block:: none

    [dimsenv] dittrich@dimsdemo1:~/dims/git/python-dimscli (develop*) $ dimscli nodes list --column Node --column Status -f shell
    b52="Agent"
    breathe="Leader"
    dimsdemo1="Agent"
    echoes="Peer"
    seamus="Peer"

..

.. _addingNewCommands:

Adding New Commands
~~~~~~~~~~~~~~~~~~~

In this example, we will add a new command ``ansible`` with a subcommand
``execute`` that will use Ansible's `Python API`_ (specifically the
``ansible.runner.Runner`` class) to execute arbitrary commands on hosts
via Ansible.

.. note::

    What is being demonstrated here is adding a new subcommand to the
    ``dimscli`` repo directly. It is also possible to add a new command
    from a module in another repo using Stevedore.

    .. TODO(dittrich) Add a cross-reference here to external module loading via Stevedore
    .. todo::
        Add a cross-reference here to external module loading via Stevedore.

    ..

..


.. _Python API: http://docs.ansible.com/ansible/developing_api.html

Here are the changes that implement this new command:

.. code-block:: diff

    commit eccf3af707aac5a13144580bfbf548b45616d49f
    Author: Dave Dittrich <dittrich@u.washington.edu>
    Date:   Fri Jan 1 20:34:42 2016 -0800

        Add 'ansible execute' command

    diff --git a/dimscli/dimsansible/__init__.py b/dimscli/dimsansible/__init__.py
    new file mode 100644
    index 0000000..e69de29
    diff --git a/dimscli/dimsansible/ansiblerunner.py b/dimscli/dimsansible/ansiblerunner.py
    new file mode 100644
    index 0000000..68cd3ea
    --- /dev/null
    +++ b/dimscli/dimsansible/ansiblerunner.py
    @@ -0,0 +1,61 @@
    +#!/usr/bin/python
    +
    +import sys
    +import logging
    +
    +from cliff.lister import Lister
    +from ansible.runner import Runner
    +
    +HOST_LIST = "/etc/ansible/hosts"
    +CMD = "/usr/bin/uptime"
    +
    +class Execute(Lister):
    +    """Execute a command via Ansible and return a list of results.
    +
    +    """
    +
    +    log = logging.getLogger(__name__)
    +
    +    def get_parser(self, prog_name):
    +        parser = super(Execute, self).get_parser(prog_name)
    +        parser.add_argument(
    +            "--host-list",
    +            metavar="<host-list>",
    +            default=HOST_LIST,
    +            help="Hosts file (default: {})".format(HOST_LIST),
    +        )
    +        parser.add_argument(
    +            "--program",
    +            metavar="<program>",
    +            default=CMD,
    +            help="Program to run (default: {})".format(CMD),
    +        )
    +        return parser
    +
    +    def take_action(self, parsed_args):
    +
    +        results = Runner(
    +                host_list=parsed_args.host_list,
    +                pattern='*',
    +                forks=10,
    +                module_name='command',
    +                module_args=parsed_args.program,
    +        ).run()
    +
    +        if results is None:
    +           print "No hosts found"
    +           sys.exit(1)
    +
    +        outtable = []
    +
    +        for (hostname, result) in results['contacted'].items():
    +            if not 'failed' in result:
    +                outtable.append((hostname, 'GOOD', result['stdout']))
    +            elif 'failed' in result:
    +                outtable.append((hostname, 'FAIL', result['msg']))
    +        for (hostname, result) in results['dark'].items():
    +            outtable.append((hostname, 'DARK', result['msg']))
    +
    +        column_names = ('Host', 'Status', 'Results')
    +
    +        return column_names, outtable
    diff --git a/setup.cfg b/setup.cfg
    index 14f6ce7..9571d4f 100644
    --- a/setup.cfg
    +++ b/setup.cfg
    @@ -37,6 +37,7 @@ dims.cli =
         files_list = dimscli.list:Files
         nodes_list = dimscli.list:Nodes
         show_file = dimscli.show:File
    +    ansible_execute = dimscli.dimsansible.ansiblerunner:Execute

     cliff.formatter.list =
         shell = dimscli.formatters.shell:DIMSShellFormatter

..

Here is what the command can do (as seen in the ``--help`` output).

.. code-block:: none

    [dimscli] dittrich@dimsdemo1:ims/git/python-dimscli/dimscli (develop*) $ dimscli ansible execute --help
    usage: dimscli ansible execute [-h]
                                   [-f {csv,html,json,json,shell,table,value,yaml,yaml}]
                                   [-c COLUMN] [--prefix PREFIX]
                                   [--max-width <integer>] [--noindent]
                                   [--quote {all,minimal,none,nonnumeric}]
                                   [--host-list <host-list>] [--program <program>]

    Execute a command via Ansible and return a list of results.

    optional arguments:
      -h, --help            show this help message and exit
      --host-list <host-list>
                            Hosts file (default: /etc/ansible/hosts)
      --program <program>   Program to run (default: /usr/bin/uptime)

    output formatters:
      output formatter options

      -f {csv,html,json,json,shell,table,value,yaml,yaml}, --format {csv,html,json,json,shell,table,value,yaml,yaml}
                            the output format, defaults to table
      -c COLUMN, --column COLUMN
                            specify the column(s) to include, can be repeated

    shell formatter:
      a format a UNIX shell can parse (variable="value")

      --prefix PREFIX       add a prefix to all variable names

    table formatter:
      --max-width <integer>
                            Maximum display width, 0 to disable

    json formatter:
      --noindent            whether to disable indenting the JSON

    CSV Formatter:
      --quote {all,minimal,none,nonnumeric}
                            when to include quotes, defaults to nonnumeric

..

The script defaults to using the standard Ansible ``/etc/ansible/hosts`` file to get its inventory. In this case,
the DIMS ``$GIT/ansible-inventory/development`` file was copied to the default location. Using this file to
execute the default command ``/usr/bin/uptime`` on the defined ``development`` hosts results in the following:

.. code-block:: none

    [dimscli] dittrich@dimsdemo1:ims/git/python-dimscli/dimscli (develop*) $ dimscli ansible execute
    +-------------------------------------+--------+------------------------------------------------------------------------+
    | Host                                | Status | Results                                                                |
    +-------------------------------------+--------+------------------------------------------------------------------------+
    | linda-vm1.prisem.washington.edu     | GOOD   |  18:31:22 up 146 days,  8:27,  1 user,  load average: 0.00, 0.01, 0.05 |
    | u12-dev-ws-1.prisem.washington.edu  | GOOD   |  18:31:21 up 146 days,  8:27,  1 user,  load average: 0.00, 0.01, 0.05 |
    | hub.prisem.washington.edu           | GOOD   |  02:31:22 up 128 days,  8:42,  1 user,  load average: 0.00, 0.01, 0.05 |
    | floyd2-p.prisem.washington.edu      | GOOD   |  18:31:21 up 20 days, 56 min,  1 user,  load average: 0.02, 0.04, 0.05 |
    | u12-dev-svr-1.prisem.washington.edu | GOOD   |  18:31:22 up 142 days, 11:22,  1 user,  load average: 0.00, 0.01, 0.05 |
    +-------------------------------------+--------+------------------------------------------------------------------------+

..

Using the ``--program`` command line option, a different command can be run:

.. code-block:: none

    [dimscli] dittrich@dimsdemo1:ims/git/python-dimscli/dimscli (develop*) $ dimscli ansible execute --program "ip addr"
    +-------------------------------------+--------+------------------------------------------------------------------------------------------------------------------+
    | Host                                | Status | Results                                                                                                          |
    +-------------------------------------+--------+------------------------------------------------------------------------------------------------------------------+
    | linda-vm1.prisem.washington.edu     | GOOD   | 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN                                              |
    |                                     |        |     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00                                                        |
    |                                     |        |     inet 127.0.0.1/8 scope host lo                                                                               |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000                          |
    |                                     |        |     link/ether 08:00:27:3b:3a:65 brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 10.0.2.15/24 brd 10.0.2.255 scope global eth0                                                           |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000                          |
    |                                     |        |     link/ether 08:00:27:36:2b:2c brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 192.168.88.11/24 brd 192.168.88.255 scope global eth1                                                   |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    | u12-dev-svr-1.prisem.washington.edu | GOOD   | 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN                                              |
    |                                     |        |     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00                                                        |
    |                                     |        |     inet 127.0.0.1/8 scope host lo                                                                               |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        |     inet6 ::1/128 scope host                                                                                     |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000                          |
    |                                     |        |     link/ether 08:00:27:38:db:8c brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 10.0.2.15/24 brd 10.0.2.255 scope global eth0                                                           |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        |     inet6 fe80::a00:27ff:fe38:db8c/64 scope link                                                                 |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000                          |
    |                                     |        |     link/ether 08:00:27:e7:80:52 brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 192.168.88.13/24 brd 192.168.88.255 scope global eth1                                                   |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        |     inet6 fe80::a00:27ff:fee7:8052/64 scope link                                                                 |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    | hub.prisem.washington.edu           | GOOD   | 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default                                |
    |                                     |        |     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00                                                        |
    |                                     |        |     inet 127.0.0.1/8 scope host lo                                                                               |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        |     inet6 ::1/128 scope host                                                                                     |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000            |
    |                                     |        |     link/ether 08:00:27:9c:f8:95 brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 10.0.2.15/24 brd 10.0.2.255 scope global eth0                                                           |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        |     inet6 fe80::a00:27ff:fe9c:f895/64 scope link                                                                 |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000            |
    |                                     |        |     link/ether 08:00:27:28:63:2a brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 192.168.88.14/24 brd 192.168.88.255 scope global eth1                                                   |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        |     inet6 fe80::a00:27ff:fe28:632a/64 scope link                                                                 |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default                      |
    |                                     |        |     link/ether 56:84:7a:fe:97:99 brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 172.17.42.1/16 scope global docker0                                                                     |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        |     inet6 fe80::5484:7aff:fefe:9799/64 scope link                                                                |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 22: veth6dc6dd5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default  |
    |                                     |        |     link/ether 8e:d6:f5:66:fb:88 brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet6 fe80::8cd6:f5ff:fe66:fb88/64 scope link                                                                |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 42: vethdc35259: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default  |
    |                                     |        |     link/ether 46:c3:87:32:83:a1 brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet6 fe80::44c3:87ff:fe32:83a1/64 scope link                                                                |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    | floyd2-p.prisem.washington.edu      | GOOD   | 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN                                              |
    |                                     |        |     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00                                                        |
    |                                     |        |     inet 127.0.0.1/8 scope host lo                                                                               |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000                          |
    |                                     |        |     link/ether 52:54:00:17:19:9a brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 172.22.29.175/24 brd 172.22.29.255 scope global eth0                                                    |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 3: eth1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN qlen 1000                                          |
    |                                     |        |     link/ether 52:54:00:85:34:b7 brd ff:ff:ff:ff:ff:ff                                                           |
    | u12-dev-ws-1.prisem.washington.edu  | GOOD   | 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN                                              |
    |                                     |        |     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00                                                        |
    |                                     |        |     inet 127.0.0.1/8 scope host lo                                                                               |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000                          |
    |                                     |        |     link/ether 08:00:27:07:6b:00 brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 10.0.2.15/24 brd 10.0.2.255 scope global eth0                                                           |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000                          |
    |                                     |        |     link/ether 08:00:27:75:a0:25 brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 192.168.88.12/24 brd 192.168.88.255 scope global eth1                                                   |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    |                                     |        | 4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN                               |
    |                                     |        |     link/ether 5a:cb:bd:c2:f5:82 brd ff:ff:ff:ff:ff:ff                                                           |
    |                                     |        |     inet 172.17.42.1/16 scope global docker0                                                                     |
    |                                     |        |        valid_lft forever preferred_lft forever                                                                   |
    +-------------------------------------+--------+------------------------------------------------------------------------------------------------------------------+

..

.. code-block:: none

    [dimscli] dittrich@dimsdemo1:ims/git/python-dimscli/dimscli (develop*) $ dimscli ansible execute --program "cat /etc/hosts"
    +-------------------------------------+--------+---------------------------------------------------------------------------------------------------------------------------------------------+
    | Host                                | Status | Results                                                                                                                                     |
    +-------------------------------------+--------+---------------------------------------------------------------------------------------------------------------------------------------------+
    | linda-vm1.prisem.washington.edu     | GOOD   | 127.0.0.1      localhost                                                                                                                    |
    |                                     |        | 127.0.1.1      ubu12-generic                                                                                                                |
    |                                     |        |                                                                                                                                             |
    |                                     |        | # The following lines are desirable for IPv6 capable hosts                                                                                  |
    |                                     |        | ::1     ip6-localhost ip6-loopback                                                                                                          |
    |                                     |        | fe00::0 ip6-localnet                                                                                                                        |
    |                                     |        | ff00::0 ip6-mcastprefix                                                                                                                     |
    |                                     |        | ff02::1 ip6-allnodes                                                                                                                        |
    |                                     |        | ff02::2 ip6-allrouters                                                                                                                      |
    |                                     |        |                                                                                                                                             |
    |                                     |        | 127.0.0.1      auth-test.prisem.washington.edu manager-test.prisem.washington.edu  reload-test.prisem.washington.edu test5.prisem.washingto |
    |                                     |        | 127.0.0.1      auth-test.prisem.washington.edu manager-test.prisem.washington.edu  reload-test.prisem.washington.edu test5.prisem.washingto |
    |                                     |        | 127.0.0.1      auth-test.prisem.washington.edu manager-test.prisem.washington.edu  reload-test.prisem.washington.edu test5.prisem.washingto |
    |                                     |        | 127.0.0.1      auth-test.prisem.washington.edu manager-test.prisem.washington.edu  reload-test.prisem.washington.edu test5.prisem.washingto |
    |                                     |        | 127.0.0.1      auth-test.prisem.washington.edu manager-test.prisem.washington.edu  reload-test.prisem.washington.edu test5.prisem.washingto |
    |                                     |        | 127.0.0.1      auth-test.prisem.washington.edu manager-test.prisem.washington.edu  reload-test.prisem.washington.edu test5.prisem.washingto |
    |                                     |        | 127.0.0.1      auth-test.prisem.washington.edu manager-test.prisem.washington.edu  reload-test.prisem.washington.edu test5.prisem.washingto |
    |                                     |        | 127.0.0.1      auth-test.prisem.washington.edu manager-test.prisem.washington.edu  reload-test.prisem.washington.edu test5.prisem.washingto |
    |                                     |        | 127.0.0.1      auth-test.prisem.washington.edu manager-test.prisem.washington.edu  reload-test.prisem.washington.edu test5.prisem.washingto |
    | u12-dev-svr-1.prisem.washington.edu | GOOD   | 127.0.0.1      localhost                                                                                                                    |
    |                                     |        | 127.0.1.1      u12-dev-svr-1                                                                                                                |
    |                                     |        |                                                                                                                                             |
    |                                     |        | # The following lines are desirable for IPv6 capable hosts                                                                                  |
    |                                     |        | ::1     ip6-localhost ip6-loopback                                                                                                          |
    |                                     |        | fe00::0 ip6-localnet                                                                                                                        |
    |                                     |        | ff00::0 ip6-mcastprefix                                                                                                                     |
    |                                     |        | ff02::1 ip6-allnodes                                                                                                                        |
    |                                     |        | ff02::2 ip6-allrouters                                                                                                                      |
    | hub.prisem.washington.edu           | GOOD   | 127.0.0.1      localhost                                                                                                                    |
    |                                     |        | 127.0.1.1      hub                                                                                                                          |
    |                                     |        |                                                                                                                                             |
    |                                     |        | # The following lines are desirable for IPv6 capable hosts                                                                                  |
    |                                     |        | ::1     localhost ip6-localhost ip6-loopback                                                                                                |
    |                                     |        | ff02::1 ip6-allnodes                                                                                                                        |
    |                                     |        | ff02::2 ip6-allrouters                                                                                                                      |
    |                                     |        | 127.0.1.1 hub                                                                                                                               |
    | floyd2-p.prisem.washington.edu      | GOOD   | 127.0.0.1      localhost                                                                                                                    |
    |                                     |        | 127.0.0.1      floyd2-p floyd2-p.prisem.washington.edu                                                                                      |
    | u12-dev-ws-1.prisem.washington.edu  | GOOD   | 127.0.0.1      localhost                                                                                                                    |
    |                                     |        | 127.0.1.1      u12-dev-1                                                                                                                    |
    |                                     |        |                                                                                                                                             |
    |                                     |        | # The following lines are desirable for IPv6 capable hosts                                                                                  |
    |                                     |        | ::1     ip6-localhost ip6-loopback                                                                                                          |
    |                                     |        | fe00::0 ip6-localnet                                                                                                                        |
    |                                     |        | ff00::0 ip6-mcastprefix                                                                                                                     |
    |                                     |        | ff02::1 ip6-allnodes                                                                                                                        |
    |                                     |        | ff02::2 ip6-allrouters                                                                                                                      |
    +-------------------------------------+--------+---------------------------------------------------------------------------------------------------------------------------------------------+

..

To run a command across the full set of ansible-compatible hosts, we can use the helper ``Makefile`` in the
``$GIT/ansible-inventory`` repo to extract a list of *all* hosts specified in *any* inventory file to
form a complete set.

.. note::

    This helper ``Makefile`` was originally written to take a set of static inventory files
    and generate a set, rather than forcing someone to manually edit a file and manually
    combine all hosts from any file (which is error prone, tedious, difficult to remember
    how to do... basically impractical for a scalable solution.)

..

.. code-block:: none

    [dimscli] dittrich@dimsdemo1:ims/git/python-dimscli/dimscli (develop*) $ (cd $GIT/ansible-inventory; make help)
    usage: make [something]

    Where 'something' is one of:

     help - Show this help information
     all -           Default is create complete_inventory file.

     inventory -     Create file 'complete_inventory' with all hosts
                     from any file with an '[all]' section in it.

     tree -          Produce a tree listing of everything except
                     'older-*' directories.

     clean -         Clean up files.


    [dimscli] dittrich@dimsdemo1:ims/git/python-dimscli/dimscli (develop*) $ (cd $GIT/ansible-inventory; make inventory)
    echo '[all]' > complete_inventory
    cat development hosts-old infrastructure Makefile prisem project | awk '\
                /^\[all\]/ { echo = 1; next; }\
                /^$/     { echo = 0; }\
                           { if (echo == 1) { print; } }' |\
                    sort | uniq >> complete_inventory

..

Now this list can be used to run the command across the full set of hosts under Ansible control.

.. code-block:: none

    [dimscli] dittrich@dimsdemo1:ims/git/python-dimscli/dimscli (develop*) $ dimscli ansible execute --host-list /home/dittrich/dims/git/ansible-inventory/complete_inventory
    +-------------------------------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    | Host                                | Status | Results                                                                                                                                                                    |
    +-------------------------------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    | rabbitmq.prisem.washington.edu      | GOOD   |  18:35:04 up 20 days,  1:00,  1 user,  load average: 0.00, 0.04, 0.05                                                                                                      |
    | wellington.prisem.washington.edu    | GOOD   |  18:35:06 up 146 days,  8:43,  1 user,  load average: 0.43, 0.64, 0.43                                                                                                     |
    | hub.prisem.washington.edu           | GOOD   |  02:35:02 up 128 days,  8:46,  1 user,  load average: 0.11, 0.06, 0.05                                                                                                     |
    | git.prisem.washington.edu           | GOOD   |  18:35:03 up 146 days,  8:30,  2 users,  load average: 0.18, 0.07, 0.06                                                                                                    |
    | time.prisem.washington.edu          | GOOD   |  18:35:04 up 20 days,  1:00,  2 users,  load average: 0.06, 0.13, 0.13                                                                                                     |
    | jira-int.prisem.washington.edu      | GOOD   |  18:35:03 up 146 days,  8:30,  2 users,  load average: 0.18, 0.07, 0.06                                                                                                    |
    | u12-dev-ws-1.prisem.washington.edu  | GOOD   |  18:35:05 up 146 days,  8:30,  1 user,  load average: 0.01, 0.02, 0.05                                                                                                     |
    | sso.prisem.washington.edu           | GOOD   |  18:35:05 up 146 days,  8:30,  1 user,  load average: 0.00, 0.02, 0.05                                                                                                     |
    | lapp-int.prisem.washington.edu      | GOOD   |  18:35:02 up 146 days,  8:31,  2 users,  load average: 0.16, 0.05, 0.06                                                                                                    |
    | foswiki-int.prisem.washington.edu   | GOOD   |  18:35:03 up 146 days,  8:31,  1 user,  load average: 0.00, 0.01, 0.05                                                                                                     |
    | u12-dev-svr-1.prisem.washington.edu | GOOD   |  18:35:03 up 142 days, 11:26,  1 user,  load average: 0.03, 0.04, 0.05                                                                                                     |
    | linda-vm1.prisem.washington.edu     | GOOD   |  18:35:05 up 146 days,  8:31,  1 user,  load average: 0.13, 0.04, 0.05                                                                                                     |
    | floyd2-p.prisem.washington.edu      | GOOD   |  18:35:02 up 20 days, 59 min,  1 user,  load average: 0.08, 0.04, 0.05                                                                                                     |
    | jenkins-int.prisem.washington.edu   | GOOD   |  18:35:03 up 146 days,  8:31,  1 user,  load average: 0.01, 0.02, 0.05                                                                                                     |
    | lapp.prisem.washington.edu          | GOOD   |  18:35:02 up 146 days,  8:31,  1 user,  load average: 0.16, 0.05, 0.06                                                                                                     |
    | eclipse.prisem.washington.edu       | DARK   | SSH encountered an unknown error during the connection. We recommend you re-run the command using -vvvv, which will enable SSH debugging output to help diagnose the issue |
    | lancaster.prisem.washington.edu     | DARK   | SSH encountered an unknown error during the connection. We recommend you re-run the command using -vvvv, which will enable SSH debugging output to help diagnose the issue |
    +-------------------------------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

..

.. note::

    As can be seen here, the hosts ``eclipse.prisem.washington.edu`` and
    ``lancaster.prisem.washington.edu`` do not conform with the standard use of
    Ansible via SSH. These kind of *one-off* or manually-configured hosts
    limit the scalability and consistent use of Ansible as a system
    configuration and management tool.

    .. TODO(dittrich): Fix these two hosts so they are in compliance.
    .. todo::

       Fix these two hosts so they are in compliance. See Jira
       ticket `DIMS-338`_.


    ..

..

.. _DIMS-338: http://jira.prisem.washington.edu/browse/DIMS-338

.. _addingModules:

Adding a Module in Another Repo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. TODO(dittrich): Clean up these notes on adding dimscli modules to repos
.. todo::

    These are just raw notes, preserved until they can be cleaned up and turned
    into meaningful instructions.

    dittrich Fri Jan  1 16:22:55 PST 2016

..

.. code-block:: none

    [dimsenv] dittrich@dimsdemo1:~/git/ansible-playbooks () $ cookiecutter https://git.openstack.org/openstack-dev/cookiecutter.git
    Cloning into 'cookiecutter'...
    remote: Counting objects: 602, done.
    remote: Compressing objects: 100% (265/265), done.
    remote: Total 602 (delta 345), reused 563 (delta 310)
    Receiving objects: 100% (602/602), 81.17 KiB | 0 bytes/s, done.
    Resolving deltas: 100% (345/345), done.
    Checking connectivity... done.
    module_name [replace with the name of the python module]: dims_ansible_playbook
    repo_group [openstack]: dims
    repo_name [replace with the name for the git repo]: ansible-playbooks
    launchpad_project [replace with the name of the project on launchpad]:
    project_short_description [OpenStack Boilerplate contains all the boilerplate you need to create an OpenStack package.]: Python ansible-playbook module for dimscli
    Initialized empty Git repository in /home/dittrich/git/ansible-playbooks/ansible-playbooks/.git/
    [master (root-commit) 7d01bbe] Initial Cookiecutter Commit.
     26 files changed, 647 insertions(+)
     create mode 100644 .coveragerc
     create mode 100644 .gitignore
     create mode 100644 .gitreview
     create mode 100644 .mailmap
     create mode 100644 .testr.conf
     create mode 100644 CONTRIBUTING.rst
     create mode 100644 HACKING.rst
     create mode 100644 LICENSE
     create mode 100644 MANIFEST.in
     create mode 100644 README.rst
     create mode 100644 babel.cfg
     create mode 100644 dims_ansible_playbook/__init__.py
     create mode 100644 dims_ansible_playbook/tests/__init__.py
     create mode 100644 dims_ansible_playbook/tests/base.py
     create mode 100644 dims_ansible_playbook/tests/test_dims_ansible_playbook.py
     create mode 100755 doc/source/conf.py
     create mode 100644 doc/source/contributing.rst
     create mode 100644 doc/source/index.rst
     create mode 100644 doc/source/installation.rst
     create mode 100644 doc/source/readme.rst
     create mode 100644 doc/source/usage.rst
     create mode 100644 requirements.txt
     create mode 100644 setup.cfg
     create mode 100644 setup.py
     create mode 100644 test-requirements.txt
     create mode 100644 tox.ini
    [dimsenv] dittrich@dimsdemo1:~/git/ansible-playbooks () $ ls -l
    total 4
    drwxrwxr-x 5 dittrich dittrich 4096 Jan  1 16:17 ansible-playbooks

..
