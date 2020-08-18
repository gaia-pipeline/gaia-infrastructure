# gaia-infrastructure

This is an infrastructure directory containing code which will be applied to the gaia deployment.

Note: Right now this is running over `gaia.cronohub.org`. So namespaces and information as such is
coded to point to that location. However, this will change once it's deployed under the proper domain.

This also means that ingress and networking and such will not be contained in this repository.

Note, that changing any files in here must be reflected under the Gaia-bot repository.

# Persistent volume

In order for flux to create a new instance while the other is running, we are using and NFS server.

Since the cluster is hosted on DigitalOcean and DO's persistent volumes are readwriteOnce we can't have
two instances at the same time using the same volume. We are circumventing this by using an NFS server.

NSF server was created using this guide: 

[NFS](https://www.digitalocean.com/community/tutorials/how-to-set-up-readwritemany-rwx-persistent-volumes-with-nfs-on-digitalocean-kubernetes)

Basically run these commands:

```
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo update
helm install nfs-server stable/nfs-server-provisioner --set persistence.enabled=true,persistence.storageClass=do-block-storage,persistence.size=20Gi
```

Then use the above persistent volume claim.

# New

The new deployment is on a single machine on Hetzner using Docker + Traefik. This deployment here represents that setting.
I'm no longer using Kubernetes.
