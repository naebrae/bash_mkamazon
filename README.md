# mkamazon

## Description

Bash script to help make a amazon/2 vagrant virtualbox box using the Amazon Linux 2 virtualbox vdi from https://cdn.amazonlinux.com/os-images/latest/

## Requirements

- vagrant
- virtualbox
- genisoimage

## Usage

1. Create the cloud-init 'seed' to create the vagrant user and add the ssh public key. Doesn't change so this is only required once if you keep the seed.iso

    ```
    ./mkamazon genseed seed.iso
    ```

2. Create a virtualbox virtual machine called 'amazon'.

    ```
    ./mkamazon create
    ```

3. Attach the downloaded Amazon Linux vdi to the 'amazon' virtual machine. Copies it first so the downloaded file is not affected.

    ```
    ./mkamazon attach /path/to/amzn2-virtualbox-2.0.20230320.0-x86_64.xfs.gpt.vdi
    ```

4. Start the virtual machine headless. Wait 5-10 minutes for the machine to boot and complete the configuration.

    ```
    ./mkamazon start seed.iso
    ```

5. Package the 'amazon' virtual machine to a vagrant box image called **amazon-virtualbox.box**. Wait 5-10 minutes for the configuration to complete before performing the 'package'

    ```
    ./mkamazon package
    ```

6. Remove the 'amazon' virtual machine.

    ```
    ./mkamazon remove
    ```

7. Add the box to vagrant

    7.1. Using a json file

    ```
    mv amazon-virtualbox.box amazon-2.0.20230320.0-virtualbox.box
    ```
    ```
    cat <<EOF > amazon.json
    {
        "name": "amazon/2",
        "description": "This box contains Amazon Linux 2",
        "versions": [
        {
            "version": "20230320.0",
            "providers": [{
               "name": "virtualbox",
               "url": "amazon-2.0.20230320.0-virtualbox.box"
            }]
        }
        ]
    }
    EOF
    ```
    ```
    vagrant box add amazon.json
    ```

    7.2. Just add the box

    > The box version will be '0'

    ```
    vagrant box add --name amazon/2 amazon-virtualbox.box
    ```
