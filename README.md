# Elemental Infra/App Demo

This demo will showcase how Elemental can repurpose Edge machines entirely and automatically based off their labels.

## Configuring an Elemental Registry Endpoint

When you create an Elemental registry endpoint, use the following cloud configuration (NOTE: you might need to update the install device setting based on the hardward):

```yaml
config:
  cloud-config:
    users:
      - name: root
        passwd: root
  elemental:
    install:
      device: /dev/sda
      poweroff: false
      reboot: true
      snapshotter:
        type: loopdevice
    reset:
      enabled: true
      reboot: true
      reset-oem: true
      reset-persistent: true
machineInventoryLabels:
  elemental-demo: enabled
```

The most important parts of this are the `machineInventoryLabels` and the reset parameters. This is what is going to enable the repurposing of edge machines.

## Creating Elemental RKE2 Clusters via Fleet GitOps

In Rancher, make sure you have an SSH secret in both `fleet-local` and `fleet-default` named `private-key` that has the ability to pull your forked repo of this. You'll need to update all `gitrepo.yaml` files to your specific gitrepo SSH URL.

In the `infra/` directory, you'll need to modify the `values.yaml` of both the Gitea and Harbor clusters:

* Update the `authConfigSecretName` setting to the secret that contains your registry credential for `rgcrprod.azurecr.us`.
* If using a different label, update the label at the end.

Once updated, commit/push. Then, on your local Rancher cluster, apply the `gitrepo.yaml` files one at a time.

## REST IS WIP