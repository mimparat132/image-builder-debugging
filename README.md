# image-builder-debugging

- Clone the image builder repository locally

```bash
git clone git@github.com:kubernetes-sigs/image-builder.git
cd image-builder
```

- Create the `proxmox.env` file with the following contents. Make sure to not use quotes or the container will not execute properly.
```bash
PROXMOX_URL=https://<pve-endpoint>:8006/api2/json
PROXMOX_USERNAME="proxmox-user"
PROXMOX_TOKEN=proxmox-user-token-secret
PROXMOX_NODE=a-proxmox-node-name
PROXMOX_ISO_POOL=your-iso-storage-pool
PROXMOX_BRIDGE=vmbr0
PROXMOX_STORAGE_POOL=your-vm-storage-pool
PACKER_FLAGS=--var 'vmid=24013201' --var 'disk_format=raw' --var 'kubernetes_rpm_version=1.32.1' --var 'kubernetes_semver=v1.32.1' --var 'kubernetes_series=v1.32' --var 'kubernetes_deb_version=1.32.1-1.1'
```
- Run the container like so to build the image:
```bash
docker run -it --rm --net=host --env-file proxmox.env \
  -v /tmp:/<your-path-here>/image-builder/images/capi/downloaded_iso_path \
  registry.k8s.io/scl-image-builder/cluster-node-image-builder-amd64:v0.1.38 build-proxmox-ubuntu-2404
```

This command assumes you are running the docker run command in the same directory you've created the `proxmox.env` file in.
