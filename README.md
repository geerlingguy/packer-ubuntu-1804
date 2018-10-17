# Packer Example - Ubuntu 18.04 minimal Vagrant Box

**Current Ubuntu Version Used**: 18.04.1

**Pre-built Vagrant Box**:

  - [`vagrant init geerlingguy/ubuntu1804`](https://vagrantcloud.com/geerlingguy/boxes/ubuntu1804)

This example build configuration installs and configures Ubuntu 18.04 x86_64 minimal using Ansible, and then generates a Vagrant box file for VirtualBox.

The example can be modified to use more Ansible roles, plays, and included playbooks to fully configure (or partially) configure a box file suitable for deployment for development environments.

## Requirements

The following software must be installed/present on your local machine before you can use Packer to build the Vagrant box file:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/) (if you want to build the VirtualBox box)
  - [Ansible](http://docs.ansible.com/intro_installation.html)

## Usage

Make sure all the required software (listed above) is installed.

Then, follow one or the other procedures below, depending on whether you want to upload the Vagrant box to Vagrant Cloud.

### With upload to Vagrant Cloud

This Packer configuration includes a post-processor that pushes the built box to Vagrant Cloud.  This requires some preparation:

  - Create a [Vagrant Cloud account](https://app.vagrantup.com/account/new).  Sign in.
    - Select _Create a new Vagrant Box_
    - Create a box named "ubuntu1804"
    - Create an access token.  Copy it to your clipboard as it will be displayed only once.
  - Create environment variable `VAGRANT_CLOUD_TOKEN` whose value is your access token:
    ```
    export VAGRANT_CLOUD_TOKEN=ab44...95ef
    ```

Change directory to the directory containing the `ubuntu1804.json` file and run Packer, specifying your Vagrant Cloud username and a new version number for this Vagrant box:

    $ packer build -var "username=yourVagrantCloudUsername" -var 'version=1.2.0' ubuntu1804.json

After a few minutes, Packer should tell you the box was generated successfully, and the box was uploaded to Vagrant Cloud.

### Skip upload to Vagrant Cloud

If you do not care to push your box to Vagrant Cloud, edit the Packer configuration file `ubuntu1804.json`:

* Remove the `vagrant-cloud` post-processor from the end of the file
* Remove the user `variables` section at the beginning of the file, so that you need not set the variables on the command line

Then, change directory to the directory containing the `ubuntu1804.json` file and run Packer:

    $ packer build ubuntu1804.json

After a few minutes, Packer should tell you the box was generated successfully.

## Testing built boxes

There's an included Vagrantfile that allows quick testing of the built Vagrant boxes. From this same directory, run the following command after building the box:

    $ vagrant up

## License

MIT license.

## Author Information

Created in 2018 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
