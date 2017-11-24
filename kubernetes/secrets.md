# Secrets

We can use **Secrets** in kubernetes to distribute credentials/keys/passwords or secret data to pods/applications

Kubernetes itself uses this **Secrets** mechanism to provide the credentials to pods to enable them to access the internal API

This is the native kubernetes mechanism, but there are other ways to provide secrets to our containers:
  - use an external vault service in your app

The following are ways in which we can use **Secrets**
  - as environment variables
  - as a file in a pod
    - this utilises **volumes** being mounted into a container
    - the volume would have files containing the secrets
    - the file would be a dotenv file for instance, or your app can just read the file
  - use an external image to pull secrets (obviously the image would be stored in a private registry :wink: )

## Generate secrets

### Using files

- `echo -n "root" > username.txt`
- `echo -n "password" > password.txt`
- `kubectl create secret generic db-user-pass --from-file=username.txt --from-file=password.txt`

You can also create secrets from SSH keys or SSL certs

- `kubectl create secret generic ssl-certificate --from-file=ssh-privatekey=~/.ssh/id_rsa --ssl-cert=ssl-cert=mysslcert.crt`

### Using YAML definitions

Create `secrets-db-secret.yml` - [Example File](secrets-db-secret.yml)
  - the username and password in the above example file are base64 encoded. To base64 encode your values, just run the following command:
    - `echo -n "my-value"|base64`

**DO NOT MISTAKE THIS FOR ENCRYPTED VALUES AS THESE ARE EASILY DECODED - DON'T STORE THESE FILES IN PLAIN TEXT IN SOURCE CONTROL**

### Now let's create our kubernetes secret

`kubectl create -f definitions/secrets-db-secret.yml`
