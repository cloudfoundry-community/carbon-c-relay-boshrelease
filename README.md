# BOSH Release for carbon-c-relay

## Usage

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_HOST
git clone https://github.com/cloudfoundry-community/carbon-c-relay-boshrelease.git
cd carbon-c-relay-boshrelease
bosh upload release releases/carbon-c-relay-1.yml
```

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest & deploy a cluster:

```
templates/make_manifest warden
bosh -n deploy
```

For AWS EC2, create a single VM:

```
templates/make_manifest aws-ec2
bosh -n deploy
```

### Override security groups

For AWS & Openstack, the default deployment assumes there is a `default` security group. If you wish to use a different security group(s) then you can pass in additional configuration when running `make_manifest` above.

Create a file `my-networking.yml`:

``` yaml
---
networks:
  - name: carbon-c-relay1
    type: dynamic
    cloud_properties:
      security_groups:
        - carbon-c-relay
```

Where `- carbon-c-relay` means you wish to use an existing security group called `carbon-c-relay`.

You now suffix this file path to the `make_manifest` command:

```
templates/make_manifest openstack-nova my-networking.yml
bosh -n deploy
```
