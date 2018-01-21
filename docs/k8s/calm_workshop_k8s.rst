**********
Kubernetes
**********

Overview
********

Kubernetes is a vendor-agnostic cluster and container management tool, open-sourced by Google in 2014. It provides a 
“platform for automating deployment, scaling, and operations of application containers across clusters of hosts”.
Above all, this lowers the cost of cloud computing expenses and simplifies operations and architecture.


Kubernetes and the Need for Containers
**************************************

Before we explain what Kubernetes does, we need to explain what containers are and why people are using those.

**Containers:**

A container is a mini-virtual machine. It is small, as it does not have device drivers and all the other 
components of a regular virtual machine. Docker is by far the most popular container and it is written in Linux. 
Microsoft also has added containers to Windows as well, because they have become so popular.

The best way to illustrate why this is useful and important is to give an example.

Suppose you want to install the nginx web server on a Linux server. You have several ways to do that. 
First, you could install it directly on the physical server’s OS. But most people use virtual machines now, 
so you would probably install it there.

But setting up a virtual machine requires some administrative effort and cost as well. And machines 
will be underutilized if you just dedicate it for just one task, which is how people typically use VMs. 
It would be better to load that one machine up with nginx, messaging software, a DNS server, etc.

The people who invented containers thought through these issues and reasoned that since nginx or any 
other application just needs some bare minimum operating system to run, then why not make a stripped down 
version of an OS, put nginx inside, and run that. Then you have a self-contained, machine-agnostic unit 
that can be installed anywhere.

Containers are so popular in todays modern datacenter, they threaten to make VMs obsolete...

**Docker Hub**


But making the container small is not the only advantage. The container can be deployed just like a VM 
template, meaning an application that is ready to go that requires little or no configuration.

There are thousands of preconfigured Docker images at the Dockerhub public repository. There, people have 
taken the time to assemble opensource software configurations that might take someone else hours or days to 
put together. People benefit from that because they can install nginx or even far more complicated items simply 
by downloading them from there.

**Example:** this one line command will down, install, and start Apache Spark with Jupyter notebooks (iPython):

.. code-block:: bash

   $ docker run -d -p 8888:8888 jupyter/all-spark-notebook

As you can see it is running on port 8888. So you could install something else on another port or even install a 
second instance of Spark and Jupyter.

Container Sprawl- The Need for Orchestration
********************************************

There's an inherent problem with containers, just like there is with virtual machines. That is the need to keep track of 
them. When public cloud companies bill you for CPU time or storage then you need to make sure you do not have any orphaned 
machines spinning out there doing nothing. Plus there is the need to automatically spin up more when a machine needs more 
memory, CPU, or storage, as well as shut them down when the load lightens.

Orchestration tackles these problems. This is where Kubernetes comes in.

Kubernetes
**********

Google built Kubernetes and has been using it for 10 years. That it has been used to run Google’s massive systems 
for that long is one of its key selling points. Two years ago Google pushed Kubernetes into open source.

Kubernetes is a cluster and container management tool. It lets you deploy containers to clusters, meaning a network
of virtual machines. It works with different containers, not just Docker.

**Kubernetes Basics**

The basic idea of Kubernetes is to further abstract machines, storage, and networks away from their physical implementation.
So it is a single interface to deploy containers to all kinds of clouds, virtual machines, and physical machines.

Here are a few of **Kubernetes** concepts to help understand what it does.

**Node**

A node is a physical or virtual machine. It is not created by Kubernetes. You create those with a cloud operating system, 
like OpenStack or Amazon EC2, or manually install them. So you need to lay down your basic infrastructure before you use 
Kubernetes to deploy your apps. But from that point it can define virtual networks, storage, etc. For example, you could use 
OpenStack Neutron or Romana to define networks and push those out from Kubernetes.

**Pods**

A pod is a one or more containers that logically go together. Pods run on nodes. Pods run together as a logical unit. So 
they have the same shared content. They all share the share IP address but can reach other other via localhost. And they can 
share storage. But they do not need to all run on the same machine as containers can span more than one machine. One node 
can run multiple pods.

Pods are cloud-aware. For example you could spin up two Nginx instances and assign them a public IP address on the Google 
Compute Engine (GCE). To do that you would start the Kubernetes cluster, configure the connection to GCE, and then type 
something like:

.. code-block:: bash

  $ kubectl expose deployment my-nginx –port=80 –type=LoadBalancer

**Deployment**

A set of pods is a deployment. A deployment ensures that a sufficient number of pods are running at one time to service 
the app and shuts down those pods that are not needed. It can do this by looking at, for example, CPU utilization.

**Vendor Agnostic**

Kubernetes works with many cloud and server products. And the list is always growing as so many companies are contributing 
to the open source project. Even though it was invented by Google, Google is not said to dominate it’s development.

To illustrate, the OpenStack process to create block storage is called Cinder. OpenStack orchestration is called Heat. You 
can use Heat with Kubernetes to manage storage with Cinder.

Kubernetes works with Amazon EC2, Azure Container Service, Rackspace, GCE, IBM Software, and other clouds. And it works with 
bare-metal (using something like CoreOS), Docker, and vSphere. And it works with libvirt and KVM, which are Linux machines 
turned into hypervisors (i.e, a platform to run virtual machines).

Use Cases
*********

So why would you use Kubernetes on, for example, Amazon EC2, when it has its own tool for orchestration (CloudFormation)? 
Because with Kubernetes you can use the same orchestration tool and command-line interfaces for all your different systems. 
Amazon CloudFormation only works with EC2. So with Kubernetes you could push containers to the Amazon cloud, your in-house 
virtual and physical machines as well, and other clouds.

Summary
*******

What is Kubernetes? It is an orchestration tool for containers. What are containers? They are small virtual machines that 
run ready-to-run applications on top of other virtual machines or any host OS. They greatly simplify deploying applications. 
And they make sure machines are fully-utilized. All of this lowers the cost of cloud subscriptions, further abstracts the 
data center, and simplifies operations and architecture. To get started learning about it, the reader can install MiniKube to 
run it all on one machine and play around with it.

