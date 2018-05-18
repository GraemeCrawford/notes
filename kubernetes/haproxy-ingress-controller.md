Features include:
  - hitless reloads
  - dynamic configuration without reloading using the HAProxy Runtime API
  - DNS records for service discovery

HAProxy’s features like dynamic scaling and reconfiguration without reloading are also very valuable in this use case as Kubernetes pods are often spawned, terminated, and migrated in quick bursts and in high amounts, especially during deployments.
