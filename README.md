# Elasticsearch APB

This [Ansible Playbook Bundle](https://github.com/ansibleplaybookbundle/ansible-playbook-bundle) deploys Elasticsearch 6.1.2 with the elasticsearch-cloud-kubernetes plugin for discovery. The deployment is done through StatefulSet.

Two plans have been defined:
* **Ephemeral** where there won't be any persistence and the Elascticsearch data will be lost upon restarts
* **Persistent** where PVCs will be created for each instance of Elasticsearch

## Configuration

* **CLUSTER_SIZE**: Number of Elasticsearch instances that will be deployed. Default `1`
* **APPLICATION_NAME**: Name of the Elasticsearch cluster that will give name also to all the objects deployed. Default `elasticsearch`
* **ES_MEMORY_LIMIT**: Memory limits to be set to the `elasticsearch` container. The half will be assigned to the Elasticsearch heap. Default `512Mi`
* **ES_PVC_SIZE**: Size of each Persistent Volume Claim that will be defined. Default `1Gi` (Only for the persistent plan)

## Ansible Service Broker - Requirements

The elasticsearch-cloud-kubernetes plugin requires permissions to `view` the project's `endpoints`. A `ServiceAccount` with a `RoleBinding` are created for that during provisioning.
In order for the `ServiceAccount` used by the APB during provisioning to be able to `create` `RoleBinding`s, the Ansible Service Broker needs to be configured with the `sandbox_role: "admin"` as showned below:

```
    openshift:
      host: ""
      ca_file: ""
      bearer_token_file: ""
      image_pull_policy: "IfNotPresent"
      sandbox_role: "admin"
      namespace: ansible-service-broker
```
