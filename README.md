# Terraform Provider for MAAS

This repository contains the source code for the Terraform MAAS provider.

## Requirements

- [Terraform](https://www.terraform.io/downloads.html) >= 0.13.x
- [Go](https://golang.org/doc/install) >= 1.16

## Build The Provider

1. Clone the repository
1. Enter the repository directory
1. Build the provider with:

    ```sh
    make build
    ```

1. (Optional): Install the freshly built provider with:

    ```sh
    make install
    ```

## Usage

### Provider Configuration

The provider accepts the following config options:

- **api_key**: [MAAS API key](https://maas.io/docs/snap/3.0/cli/maas-cli#heading--log-in-required).
- **api_url**: URL for the MAAS API server (eg: <http://127.0.0.1:5240/MAAS>).
- **api_version**: MAAS API version used. It is optional and it defaults to `2.0`.

#### `maas`

```hcl
provider "maas" {
  api_version = "2.0"
  api_key = "YOUR MAAS API KEY"
  api_url = "http://<MAAS_SERVER>[:MAAS_PORT]/MAAS"
}
```

### Resource Configuration

#### `maas_instance`

It is used to deploy and release machines already registered and configured in MAAS based on the specified parameters. If no parameters are given, a random machine will be allocated and deployed using the defaults.

##### Example

```hcl
resource "maas_instance" "two_random_nodes_2G" {
  count = 2
  min_cpu_count = 1
  min_memory = 2048
}
```

##### Available Parameters

| Name | Type | Required | Description
| ---- | ---- | -------- | -----------
| `min_cpu_count` | `string` | `false` | The minimum CPU cores count used for MAAS machine allocation.
| `min_memory` | `int` | `false` | The minimum RAM memory (in MB) used for MAAS machine allocation.
| `tags` | `[]string` | `false` |List of tag names used for MAAS machine allocation.
| `zone` | `string` | `false` | Zone name used for MAAS machine allocation.
| `pool` | `string` | `false` | Pool name used for MAAS machine allocation.
| `distro_series` | `string` | `false` | Distro series used to deploy the MAAS machine. It defaults to `focal`.
| `hwe_kernel` | `string` | `false` | Hardware enablement kernel to use with the image. Only used when deploying Ubuntu.
| `user_data` | `string` | `false` | Cloud-init user data script that gets run on the machine once it has deployed.