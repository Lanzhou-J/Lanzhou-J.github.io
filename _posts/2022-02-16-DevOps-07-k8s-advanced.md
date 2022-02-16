---
title: DevOps 07 - Kubernetes advanced
layout: post
subtitle: null
date: '2022-02-16 23:30:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- study
- English
- wiki
- devops
---
**Research summary:**

- Advanced concepts
  - secrets
  - configmaps
  - volumes
- Templating

## Volume

What is volume:

- Kubernetes supports many types of volumes. A [Pod](https://kubernetes.io/docs/concepts/workloads/pods/) can use any number of volume types simultaneously. Ephemeral volume types have a lifetime of a pod, but persistent volumes exist beyond the lifetime of a pod.
- At its core, a volume is a directory, possibly with some data in it, which is accessible to the containers in a pod. How that directory comes to be, the medium that backs it, and the contents of it are determined by the particular volume type used.

### 1. Basic storage

- **EmptyDir**
    - An `emptyDir` volume is first created when a Pod is assigned to a node, and exists as long as that Pod is running on that node. As the name says, the `emptyDir` volume is initially empty. All containers in the Pod can read and write the same files in the `emptyDir` volume, though that volume can be mounted at the same or different paths in each container. When a Pod is removed from a node for any reason, the data in the `emptyDir` is deleted permanently.
    
    ```yaml
    volumes:
      - name: cache-volume
        emptyDir: {}
    ```
    
- **HostPath**
    - A `hostPath` volume mounts a file or directory from the host node's filesystem into your Pod. This is not something that most Pods will need, but it offers a powerful escape hatch for some applications.
        
    - HostPath volumes present many security risks, and it is a best practice to avoid the use of HostPaths when possible. When a HostPath volume must be used, it should be scoped to only the required file or directory, and mounted as ReadOnly.
    
    ```yaml
    volumes:
      - name: test-volume
        hostPath:
          # directory location on host
          path: /data
          # this field is optional
          type: Directory
    ```
    
- **NFS**
    - An `nfs` volume allows an existing NFS (Network File System) share to be mounted into a Pod. Unlike `emptyDir`, which is erased when a Pod is removed, the contents of an `nfs` volume are preserved and the volume is merely unmounted. This means that an NFS volume can be pre-populated with data, and that data can be shared between pods. NFS can be mounted by multiple writers simultaneously.
    - **Note: You must have your own NFS server running with the share exported before you can use it.**

### 2. Advanced storage

- **PV & PVC**
    
    - PV(Persisted volume) → abstraction, cross namespace
    - PVC(Persisted volume Claim) → apply for resource
    - PV + PVC → responsibility separation
        - storage: managed by storage engineer
        - PV: managed by Kubernetes managers
        - PVC: manged by kubernetes users
    
- **Life Cycle**
    - resource provisionning
    - resource binding
    - resource using
    - resource release
    - resource reclaim

### 3. Configuration storage

- **ConfigMap**
    - We can use a `ConfigMap` to store application configuration as key-value pairs. This configuration can be consumed by pods as environment variables, command-line arguments, or volume files. Confidential data should not be stored in a `ConfigMap` and should be instead stored as a `Secret`.
    - How to use configMap
        - ConfigMap provides a way to inject configuration data into pods. The data stored in a ConfigMap can be referenced in a volume of type configMap and then consumed by containerized apps running in your pod.
        - Mounted ConfigMaps are updated automatically.
        - **A container using a ConfigMap as a [subPath](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) volume mount will not receive ConfigMap updates.?**
    
- **Secret**
    - Secrets are a first-class citizen in Kubernetes. They are mounted as data volumes or environment variables and are specific to a particular namespace, ensuring that the scope isn’t across all applications.
    - kubelet process is responsible for mounting volumes and secrets in the worker node.
    - **kubelet**: The `kubelet` is an agent running on each node in the cluster. It watches for pod assignments to the node, executes pod containers using the container engine, manages pod volumes and secrets, and runs status health checks against pods and the node.
    - **Secrets**: Applications may require data that is more sensitive in nature, database passwords or API tokens. These should not be exposed to everyone, as would be the case if kept in the yaml. When we create a `Secret`, the values are encoded and are no longer visible in plaintext. We can use the `Secret` value in a similar way to that of values in a `ConfigMap`, but the value is only exposed to the consuming pod.
    - Kubernetes Secrets are, by default, stored unencrypted in the API server's underlying data store (etcd). (Encrypted now according to Phil)Anyone with API access can retrieve or modify a Secret, and so can anyone with access to etcd. Additionally, anyone who is authorized to create a Pod in a namespace can use that access to read any Secret in that namespace; this includes indirect access such as the ability to create a Deployment.

### Templating

It is generally good practice to template your infrastructure code - this way, you can make sure that your different environments are using the same setup.

For this, we recommend using [ktmpl](https://github.com/jimmycuadra/ktmpl) - a templating engine that allows you to set parameters as you need.

`ktmpl $(file) -f params/defaults.yaml | kubectl apply -f -`

- If your [workflow template](https://cloud.google.com/dataproc/docs/concepts/workflows/overview) will be run multiple times with different values, you can avoid having to edit the workflow each time by defining parameters in the template (parameterizing the template). Then, you can pass different values for the parameters each time you run the template. (from Google Dataproc)
- A template is a pattern used for making accurate copies of something. A template engine is software designed to combine templates with a data model. → to produce multiple pages that share the same look throughout the site.(website development, twig for PHP)

Other use cases:

- CloudFormation
- Jekyll template
    - Jekyll has an extensive theme system that allows you to leverage community-maintained templates and styles to customize your site's presentation.

Templating engine:

- Gomplate
    - `gomplate` is a template renderer which supports a growing list of datasources, such as: JSON (*including EJSON - encrypted JSON*), YAML, AWS EC2 metadata, [BoltDB](https://pkg.go.dev/go.etcd.io/bbolt), [Hashicorp Consul](https://www.consul.io/) and [Hashicorp Vault](https://www.vaultproject.io/) secrets.
- ktmpl - simple templating tool
- Helm: templated yaml

---
#### Useful Resources:
