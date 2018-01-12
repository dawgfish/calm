********************
Calm Deep Dive
********************

app_blueprints
**************

AppBlueprint will be derived from Resource class and all API calls will be procedural in nature.
Users will be able to create, edit and delete app_blueprints. The blueprints can be in various states while it is being created/edited. Its various states can be Draft,  Compiled,  Active, InActive,  Deleted,  Error. Once the blueprint is successfully compiled and verified it can be launched to create an App.

**app_blueprints → app.spec Generation**

When the blueprint is launched using POST /app_blueprints/{uuid}/launch, following things happen:

1. Styx will receive the blueprint (Say BlueprintA) from which the application (say AppB) is to be instantiated.

2. Blueprint-A will be cloned with a new name and uuid - Blueprint-A_for_AppB

3. Styx will then generate the app spec from BlueprintA_for_AppB

4. Styx will post locking Intentful call to Aplos POST /apps

5. Aplos will forward the call to Aplos Engine where the app plugin will in turn forward it to Styx

6. Styx will make an create action call to Jove

**app.spec parsing and runbook generation**

Jove will generate the app spec and runbooks for its entities. It does so by carrying out the following:

1. Jove populates the user inputs within BlueprintA_for_AppB

2. Generate live entities within IDF

3. Parse the spec for resolving macros

4. Generates dependencies, implicit and explicit

5. Generates runbooks for all system actions or invokes the create runbook already existing (This create runbook is generated in case the blueprint is compiled) 

**Runbook → Epsilon workflow generation**

The following steps be carried out by the workflow generator:

1. It would be to create entities with Epsilon for App, Deployments, Substrates, Packages and Actions

2. Generates the workflows from the actions for Epsilon to execute

3. Creates a run log entry into IDF for execution of the action

4. Initiate execution of the workflow for the corresponding action in Epsilon. 

**Note:** The assumption here is that epsilon in PCVM would be able to reach the VMs in all deployments. This needs more discussion with the infra team.

**LCA Execution** 

LCA Execution involves Epsilon and Iris: 

1. Epsilon will begin execution of the workflow. 

2. Epsilon will regularly send updates back to Iris (Runlog listener) at various stages of workflow execution. Especially when attributes or properties are needed to be set for Calm entities.

3. Iris in turn will update the run logs to indicate the progress of the action.

4. Iris will update the attributes and state of the app and its associated entities in IDF.

5. Once Iris receives a notification that workflow is complete it will update the spec for the entity in IntentSpec and unlock the app and its entities.

6. Epsilon has to store task output on a shared filesystem provided by the platform. The filenames would have to be generated and stored using UUIDs. Later, once the platform has ElasticStack, epsilon would push the outputs there.
 

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
|                                          |                                                        |
+------------------------------------------+--------------------------------------------------------+
