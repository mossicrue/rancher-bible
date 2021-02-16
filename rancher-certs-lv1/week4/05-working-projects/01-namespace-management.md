# Namespace Management
You can move a namespace to a different project from the top-level cluster menu where Projects are defined.

Any resource configuration that was attached to the namespace is released and the new projects resources configuration is applied.

If the destination project does not have a quota, but the origin project does, then the quota that was attached to the namespace is released.

It is not possible to move a namespace into a project that already has a resource quota configured.

## Move a namespace

- Navigate to the "Cluster" details page using the Cluster Manager dropdown menu and click on the "Projects/Namespaces" link

- Create the new project where move the namespace, if not already present

- Then, select the namespace you want move and click on the "Move" button at the top of the page

- Now, in the prompt dialog box, choose the new project where move the namespace
