# Instructions

[image ocids]:https://docs.cloud.oracle.com/iaas/images/oraclelinux-7x/
[oci]: https://cloud.oracle.com/cloud-infrastructure
[oci console]: https://console.us-ashburn-1.oraclecloud.com/
[terraform]: https://www.terraform.io
[terraform download]: https://www.terraform.io/downloads.html
[terraform options]: ./terraformoptions.md

## Install Terraform

1. [Download Terraform][terraform download]. You need version 0.11.10

2. Extract the terraform binary to a location in your path

    ```
    $ unzip terraform_0.11.10_linux_amd64.zip
    $ sudo cp terraform /usr/local/bin
    $ terraform -v
    Terraform v0.11.10
    ```

## Generate ssh keys

Generate an ssh key:

```
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/oracle/.ssh/id_rsa): /home/oracle/test/oci_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/oracle/test/oci_rsa.
Your public key has been saved in /home/oracle/test/oci_rsa.pub.
The key api_fingerprint is:
SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx oracle@redwood
```

> N.B. Replace 'oracle' above by your username

## Generate API keys

1. Create a .oci directory:

    ```
    $ mkdir ~/.oci
    ```

2. Generate the API private key

    ```
    $ openssl genrsa -out ~/.oci/oci_api_key.pem -aes128 2048
    ```

3. Ensure that only you can read the private key file:

    ```
    $ chmod go-rwx ~/.oci/oci_api_key.pem
    ```

4. Generate the public key:

    ```
    $ openssl rsa -pubout -in ~/.oci/oci_api_key.pem -out ~/.oci/oci_api_key_public.pem
    ```

## Configure your OCI account to use Terraform

1. Open the oci_api_key_public.pem file in a text editor and copy its content

2. Login to [OCI console][oci console]

3. Click on the username (top navigation) and select 'User Settings'

4. Under 'API Keys', Click on 'Add Public Key'

5. Paste the contents of the oci_api_key_public.pem file. Click 'Add'

6. You'll see the fingerprint of your ssh key. You'll copy this in the next section.

## Configure up your environment to create the ociatlas

1. Copy the terraform.tfvars.example file

    ```
    $ cp terraform.tfvars.example terraform.tfvars
    ```

2. Open the terraform.tfvars in a text editor e.g. vi, nano, emacs etc.

3. Copy the tenancy OCID from the OCI Console (Menu > Administration > Tenancy Details) and paste it in the tenancy_ocid field in terraform.tfvars e.g.:

    ```
    tenancy_ocid = "ocid1.tenancy.xx..xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" 
    ```

4. Under the 'User Information' tab, locate 'OCID' and click on 'Copy'. Paste it in the user_ocid field e.g.

    ```
    user_ocid = "ocid1.user.xx..xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    ```

5. Copy the compartment OCID from the OCI Console (Menu > Identity > Compartments) and paste it in the compartment_ocid field in terraform.tfvars e.g.:

    ```
    compartment_ocid = "ocid1.compartment.xx..xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" 
    ```

6. Copy the fingerprint of your api key from the OCI Console and paste its value in the api_fingerprint field in terraform.tfvars e.g.

    ```
    api_fingerprint = "xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx"
    ```

7. Add the path to the following keys (based on example above):

    |   key   | path   |
    |   ----  | ----   |
    | api_private_key_path| ~/.oci/oci_api_key.pem |
    | ssh_private_key_path| /home/oracle/.ssh/id_rsa |
    | ssh_public_key_path | /home/oracle/.ssh/id_rsa.pub |


6. Set your region e.g.

    ```
    region = "us-ashburn-1"
    ```

7. Set the following environment variables:

    ```
    export http_proxy=http://proxy.server.com:port/
    export https_proxy=http://proxy.server.com:port/
    ```

> N.B. Replace the proxy.server.com:port with your proxy server address and port. (if you have one)
