# Elasticsearch APB

[![Build Status](https://travis-ci.org/ansibleplaybookbundle/es-apb.svg?branch=master)](https://travis-ci.org/ansibleplaybookbundle/es-apb) [![License](https://img.shields.io/:license-Apache2-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0)

![elasticsearch image](./docs/imgs/elasticsearch_logo.jpg)

This [Ansible Playbook Bundle](https://github.com/ansibleplaybookbundle/ansible-playbook-bundle) deploys Elasticsearch with the elasticsearch-cloud-kubernetes plugin for discovery. The deployment is done through StatefulSet.

Two plans have been defined:
* **Ephemeral** where there won't be any persistence and the Elascticsearch data will be lost upon restarts
* **Persistent** where PVCs will be created for each instance of Elasticsearch

## Configuration

* **cluster_size**: Number of Elasticsearch instances that will be deployed. Default `1`
* **application_name**: Name of the Elasticsearch cluster that will give name also to all the objects deployed. Default `elasticsearch`
* **es_memory_limit**: Memory limits to be set to the `elasticsearch` container. The half will be assigned to the Elasticsearch heap. Default `512Mi`
* **es_pvc_size**: Size of each Persistent Volume Claim that will be defined. Default `1Gi` (Only for the persistent plan)
* **es_version**: Dropdown to select the Elasticsearch version to deploy

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

**NOTE**: Make sure your Ansible Service Broker is running with the property `launch_apb_on_bind: true` if you want to be able to customize the environment variable
dynamically during bind

## Host Requirements

https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html
sudo sysctl -w vm.max_map_count=262144
