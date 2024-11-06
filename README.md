# Bare Metal Operator

The Bare Metal Operator implements a Kubernetes API for managing bare metal
hosts. It maintains an inventory of available hosts as instances of the
`BareMetalHost` Custom Resource Definition.

Please, see the [upstream project](https://github.com/metal3-io/baremetal-operator)
for more information.

## How to add a new upstream CRD to openshift

### Step 1: add a new kubebuilder RBAC directive to cluster-baremetal-operator

In [cluster-baremetal-operator](https://github.com/openshift/cluster-baremetal-operator), in
`provisioning_controller.go` there is a long list of RBAC directives and you need to register your new CRD there.

For example,

``` go
// +kubebuilder:rbac:groups=metal3.io,resources=hostupdatepolicies,verbs=get;list;watch;create;update;patch;delete
// +kubebuilder:rbac:groups=metal3.io,resources=hostupdatepolicies/status,verbs=get;update;patch
// +kubebuilder:rbac:groups=metal3.io,resources=hostupdatepolicies/finalizers,verbs=update
```

Then, regenerate manifests with:

```
$ make manifests
```

This PR to cluster-baremetal-operator will block your PR to bring the new CRDs to baremetal-operator.

### Step 2: add an entry to ocp kustomization

In `config/crd/ocp/ocp_kustomization.yaml`, add your new CRD to the list of resources.

``` diff
resources:
  - bases/metal3.io_baremetalhosts.yaml
  - bases/metal3.io_hostfirmwaresettings.yaml
  - bases/metal3.io_hostfirmwarecomponents.yaml
  - bases/metal3.io_firmwareschemas.yaml
  - bases/metal3.io_preprovisioningimages.yaml
  - bases/metal3.io_bmceventsubscriptions.yaml
  - bases/metal3.io_hardwaredata.yaml
  - bases/metal3.io_dataimages.yaml
+ - bases/metal3.io_hostupdatepolicies.yaml
```
