Tear down
To undo what kubeadm did, you should first drain the node and make sure that the node is empty before shutting it down.

Talking to the master with the appropriate credentials, run:

`kubectl drain <node name> --delete-local-data --force --ignore-daemonsets`
`kubectl delete node <node name>`

Then, on the node being removed, reset all kubeadm installed state:

`kubeadm reset`


If you wish to start over simply run kubeadm init or kubeadm join with the appropriate arguments.

More options and information about the kubeadm reset command.
