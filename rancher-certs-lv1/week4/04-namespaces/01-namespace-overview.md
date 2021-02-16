# Namespace Overview

In Kubernetes, a namespace is a logical seperation of resources.  
It helps keep resources organized, and it allows resources of the same type with the same name to coexist on the same cluster.

Kubernetes describes Namespaces as "virtual clusters within a physical cluster," and Namespaces are the boundary at which administrators apply role-based access control and broad resource constraints.

Rancher also applies RBAC policies to namespaces, but it includes an additional structure called a Project, which is a collection of namespaces.  
Work done at the Project level applies to all Namespaces that are members of the group.
