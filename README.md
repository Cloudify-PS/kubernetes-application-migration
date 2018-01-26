# kubernetes-application-migration
Example of moving nodecellar kubernetes application from one kuberentes node to another. Uses cloudify-kubernetes-plugin, cloudify-utilities-plugin (configuration) and configuration_update workflow


Inputs:
- kubernetes_configuration_file_content: Configuration of the kubernetes master. It can be retrieved using command:
```
kubectl config view --raw
```
- external_ip: IP for the end user/Floating IP. Should be one of the Kubernetes VM interfaces
- external_port: port for the end-user access. Default is 8080
- parameters_json: node_lab``el for an update. Example:
```
parameters_json:
  default:
    node_label:
      demo: node1
```

```
cfy install blueprint.yaml -d demo
```

```
cfy execution start configuration_update -d demo -p '{"node_types_to_update": ["MovableKubernetesResourceTemplate"], "params": {"node_label": {"demo": "node2"}}, "configuration_node_type": "configuration_loader"}'
cfy execution start configuration_update -d demo -p '{"node_types_to_update": ["MovableDeployment"], "params": {"node_label": {"demo": "node2"}}, "configuration_node_type": "configuration_loader"}'
```

```
cfy uninstall demo
```

Labeling K8S node
```
kubectl label nodes <node_nameX> demo=nodeX
```