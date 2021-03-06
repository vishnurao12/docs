page_main_title: Amazon ECS Using custom task definition templates
main_section: Deploy
sub_section: Amazon ECS

# Customizing your Amazon ECS Task Definitions

Task definitions are loaded with features to fit various scenarios. This page will describe how you can set up some of the more advanced sections in your Shippable pipeline when deploying using the managed [deploy job](/platform/jobs-deploy/).

Task definitions are fully customizable through the `dockerOptions` type resource, so you'll need one of those:
```
resources:

- name: deploy-ecs-lb-docker-options
  type: dockerOptions
  version:
      # add settings here

```
You can also look at the complete reference for this type of resource [here](../platform/resource-dockeroptions).

## Managed

The `dockerOptions` resource is made to be used with Shippable managed jobs including `manifest` and `deploy`.  It can be applied to an entire manifest, multiple manifests, or even a single image within a manifest.  Many of the core service and task definition options are directly modifiable through certain keywords in the dockerOptions.  Here's a table of the features we directly support.  The left column is how the option is added to the `dockerOptions` resource, and the right column is the setting that it maps to on Amazon ECS.

| Shippable Tag | Amazon ECS |
|-|-|
| **memory** | memory |
| **cpuShares**  | cpu|
| **portMappings** | portMappings|
| **links**  | links|
| **hostName** | hostname|
| **user** | user|
| **labels** | dockerLabels|
| **cmd** | command |
| **entryPoint** | entryPoint|
| **volumes** | TOP LEVEL -> volumes/mountPoints |
| **networkDisabled** | disableNetworking|
| **privileged** | privileged |
| **readOnlyRootFilesystem** | readonlyRootFilesystem |
| **dnsServers** | dnsServers |
| **dnsSearch** | dnsSearchDomains |
| **extraHosts** | extraHosts |
| **volumesFrom** | volumesFrom |
| **network** | TOP LEVEL -> networkMode |
| **ulimits** | ulimits |
| **securityOptions** | dockerSecurityOptions |
| **logConfig** | logConfiguration |
| **memoryReservation**  [ in MB ]  | memoryReservation |
| **workingDir** | workingDirectory |
| **essential** | essential |


If the setting that you're looking for isn't here, don't worry! We also natively support many of the provider-specific fields.  These are added to special sections of your `dockerOptions` named after the object you're trying to update.

There are two special sections for Amazon ECS: `service` and `taskDefinition`. Please refer to the following yml to find the supported options.
```
  resources:
  - name: <string>
    type: dockerOptions
    version:
      service:
        loadBalancer:
         - <object>
        desiredCount: <number>
        clientToken: <string>
        role: <string>
        deploymentConfiguration:
          maximumPercent: <number>
          minimumHealthyPercent: <number>
        placementStrategy:
         - field: <string>
           type: <string>
         - field: <string>
           type: <string>
      taskDefinition:
        family: <string>
        taskRoleArn: <string>
        networkMode: <string>
        volumes:
          - "<source>:<container path>:<options>"
          - "<source>:<container path>:<options>"
```
