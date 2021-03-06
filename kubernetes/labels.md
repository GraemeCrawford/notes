## Labels

Key/value pairs that can be attached to objects
  - like tags in AWS etc, used to tag resources

You can label your objects, ie pods, following an organizational structure
  - **Key**: environment - **Value** dev / staging / qa / prod
  - **Key**: department - **Value** IT / HR / marketing

This way you will know which environment your pods belong to.

#### Example:

```
metadata:
  name: nodehelloworld.example.com
  labels:
    app: helloworld
```

- Labels don't need to be unique. You can have multiple labels for one object if you wish
- You can use filters to narrow down results
  - using label selectors

- Using Label Selectors you can use matching expressions to match labels
  - eg. a certain pod can only run on a node labeled with
    - "environment" equals "development"
    - "environment" in "development" or "qa"


### Node Labels

Once nodes are tagged, you can use label selectors in definition files to only allow pods to run on specific sets of nodes

There are two steps required to achieve this:
  - first you must tag the node
  - then add a nodeSelector to your pod configuration (definition file)

Let's check the labels assigned to our nodes:
  - `kubectl get nodes --show-labels`

To assign a label to a node, first we want to get the name of the node we want to label:
  - `kubectl get nodes`

Then we assign the label to the desired node:
  - `kubectl label nodes <node_name> key=value`

For example: `kubectl label nodes node1.eu-west-1.compute.internal environment=development`

Now that we have the node labeled with our key value pair (environment=development), we can add our nodeSelector to our deployment definition file:

  - under the `container:` section we just need to add:
    ```
      nodeSelector:
        environment: development
    ```

Now when we create our deployment `kubectl create -f /definitions/helloworld-deployment.yml`, our pods will only be created on the nodes with labels that match our `nodeSelector` key/value pair.

You can confirm this by describing the pod `kubectl describe pod <pod_name>`
