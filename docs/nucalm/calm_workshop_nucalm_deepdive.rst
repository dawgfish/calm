********************
Calm Deep Dive
********************

Macros
******

**Syntax**

For attribute/property access of other objects:
                @@{<Object-Name>.<variable/attribute name>}@@

Ex: @@{my_vm.vm_ip}@@. Fetch “vm_ip” property from “my_vm” object which may either be any one of the supported object types.

Supported Objects

- Application
- Deployment
- Service
- Package
- Substrate

Property access across applications is not supported for now. 

For attribute/property access of parent objects:
                 @@{<Object-Type>.<variable/attribute name>}@@
                 

**Ex:** @@{Deployment.vm_ip}@@

Fetch “vm_ip” from the deployment on which the current script is being executed without having to know the name of the deployment directly.

Built-in macros
***************

Similar to the previous implementation, prefix can be changed.

+------------------------------------------+--------------------------------------------------------+
| @@{calm_application_uuid}@@              | UUID of the application                                |
+------------------------------------------+--------------------------------------------------------+
| @@{calm_project_name}@@                  | Name of the project that the application belongs to    |
+------------------------------------------+--------------------------------------------------------+
| @@{calm_is_within("time1", "time2")}@@   | Return '1' if the current time is within the supplied  |
|                                          | time1 and time2 range                                  | 
+------------------------------------------+--------------------------------------------------------+
