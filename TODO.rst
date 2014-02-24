==========
TODO tasks
==========

SOP for setting up a new Jenkins slave:
=======================================

Part A: On the new host:
------------------------

1) Log into the new host via SSH.

2) Get root.

3) Add Jenkins slave user account::

     useradd jenkins-build

4) Install SSH key::

     mkdir /home/jenkins-build/.ssh

     chmod 700 /home/jenkins-build/.ssh

     Copy /home/jenkins-build/.ssh/authorized_keys from an existing Jenkins slave.

     chmod 600 /home/jenkins-build/.ssh/authorized_keys

     chown -R jenkins-build:jenkins-build /home/jenkins-build/.ssh

5) Install build dependency packages::

     yum install git mock rpmbuild

6) Add Jenkins slave user account to "mock" group in /etc/group::

     usermod -a -G mock jenkins-build

Ideally we will manage these steps via Ansible going forward.


Part B: In Jenkins' web interface:
----------------------------------

1) Log into Jenkins

2) Click "Manage Nodes"

3) Add a new node

The new node should appear as online and ready for service.

Part C: Installing GPG keys
---------------------------

Once the host is configured and online in Jenkins, we need to be sure that it
also has the "release" and "autobuild" GPG keys available. We can su to the
jenkins-build user and utilize its preexistin SSH key in order to make the
transfer easier.

On the jenkins.front.sepia.ceph.com server, as root::

     su - jenkins-build

     rsync -Pa build/gnupg.autobuild gitbuilder-kernel-rpm-rhel7beta-amd64-basic:/home/jenkins-build/build

     rsync -Pa build/gnupg.ceph-release gitbuilder-kernel-rpm-rhel7beta-amd64-basic:/home/jenkins-build/build
