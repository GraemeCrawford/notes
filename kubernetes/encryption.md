Configuration:

**By default the kubernetes config ( yaml ) files are in the /etc/kubernetes/manifests directory.**




```
kind: EncryptionConfig
apiVersion: v1
resources:
  - resources:
    - secrets
    providers:
    - identity: {}
    - aesgcm:
        keys:
        - name: key1
          secret: c2VjcmV0IGlzIHNlY3VyZQ==
        - name: key2
          secret: dGhpcyBpcyBwYXNzd29yZA==
    - aescbc:
        keys:
        - name: key1
          secret: c2VjcmV0IGlzIHNlY3VyZQ==
        - name: key2
          secret: dGhpcyBpcyBwYXNzd29yZA==
    - secretbox:
        keys:
        - name: key1
          secret: YWJjZGVmZ2hpamtsbW5vcHFyc3R1dnd4eXoxMjM0NTY=
```

Each **resources** array item is a separate config and contains a complete configuration.
The resources.resources field is an array of Kubernetes resource names (resource or resource.group) that should be encrypted.
The providers array is an ordered list of the possible encryption providers, and only one provider type may be specified per entry. Identity or aesbc may be provided, but not both in the same item.


#### An example encryption config file:

Create the following file somewhere sensible - perhaps **/etc/kubernetes/manifests/encryption.yaml**

```
kind: EncryptionConfig
apiVersion: v1
resources:
  - resources:
    - secrets
    providers:
    - aescbc:
        keys:
        - name: key1
          secret: <BASE 64 ENCODED SECRET>
    - identity: {}
```

##### Steps to create a new secret

Generate a 32 byte random key and base64 encode it. If you’re on Linux or Mac OS X, run the following command:

- ```head -c 32 /dev/urandom | base64```

- Place the value outputted by the above command into the **secret** field in the encryption config file

- Set the **--experimental-encryption-provider-config** flag in the kube-apiserver config file in the following section:

  ```
  spec:
  containers:
  - command:
    - --experimental-encryption-provider-config=
  ```

  - /etc/kubernetes/manifests/kube-apiserver.yaml
    - ```--experimental-encryption-provider-config=/etc/kubernetes/manifests/encryption.yaml```
    

  - Restart the API server



.

.

.

.

.

.





#### End
