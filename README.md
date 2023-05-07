# Ansible — Nginx

This playbook installs [Nginx](https://nginx.org/en/) on a Debian/Ubuntu instance which will handle the HTTPS connection from the browser. [LetsEncrypt](https://letsencrypt.org/) is used to create the HTTPS certificate.

**Assumption:** Instance deployed using the Terraform script below,
* terraform__oci-instance-3
	* GitHub: [github.com/k3karthic/terraform__oci-instance-3](https://github.com/k3karthic/terraform__oci-instance-3)
	* Codeberg: [codeberg.org/k3karthic/terraform__oci-instance-3](https://codeberg.org/k3karthic/terraform__oci-instance-3)

## Code Mirrors

* GitHub: [github.com/k3karthic/ansible__nginx](https://github.com/k3karthic/ansible__nginx/)
* Codeberg: [codeberg.org/k3karthic/ansible__nginx](https://codeberg.org/k3karthic/ansible__nginx)

## Requirements

Install the following before running the playbook,
```
pip install oci
ansible-galaxy collection install oracle.oci
```

## Dynamic Inventory

The Oracle [Ansible Inventory Plugin](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/ansibleinventoryintro.htm) populates public Ubuntu instances.

All target Ubuntu instances must have the freeform tag `nginx_service: yes`.

## Configuration

1. Create an inventory file for the instance in `inventory/group_vars/`. Use `inventory/group_vars/tag_njalla_domain_id=598115.yml.sample` as a reference. Specify the following variables,

	2. `domain_name`: The domain name assigned to the instance
	3. `letsencrypt_email`: Your email address to register with LetsEncrypt
	4. `ansible_ssh_private_key_file`: Path to SSH keypair for instance
3. Update `inventory/oracle.oci.yml`,
    1. Specify the region where you have deployed your server on Oracle Cloud. List of regions are at [docs.oracle.com/en-us/iaas/Content/General/Concepts/regions.htm](https://docs.oracle.com/en-us/iaas/Content/General/Concepts/regions.htm).
    1. Configure the authentication as per the [Oracle Guide](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/sdkconfig.htm#SDK_and_CLI_Configuration_File)
4. Set username and ssh authentication in `inventory/group_vars/all.yml`

## Deployment

Run the playbook using the following command,
```
./bin/apply.sh
```

## Encryption

Encrypt sensitive files (SSH private key and `inventory/group_vars/tag_njalla_domain_id=598115.yml`) before saving them. `.gitignore` must contain the unencrypted file paths.

Use the following command to decrypt the files after cloning the repository,

```
$ ./bin/decrypt.sh
```

Use the following command after running terraform to update the encrypted files,

```
$ ./bin/encrypt.sh <gpg key id>
```
