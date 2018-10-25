# Packer Example - Ubuntu 18.04 minimal Vagrant Box

**Current Ubuntu Version Used**: 18.04.1

This example build configuration installs and configures Ubuntu 18.04 x86_64 minimal using Ansible, generates a Vagrant box file for VirtualBox, and uploads the box to Vagrant Cloud.

You can modify the example to use more Ansible roles, plays, and included playbooks to fully configure (or partially) configure a box file suitable for deployment for development environments.

## Requirements

The following software must be installed on your local machine before you can use Packer to build the Vagrant box file:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/)
  - [Ansible](http://docs.ansible.com/intro_installation.html)
  - nfsd.  Be careful to not accidentally also install NIS--in the absence of an NIS server, this will cause your computer to become very slow.

## Usage with upload to Vagrant Cloud

If you would rather not upload to Vagrant Cloud, see [Usage without upload to Vagrant Cloud](#usage-without-upload-to-vagrant-cloud), below.

### Build
This Packer configuration includes a post-processor that pushes the built box to Vagrant Cloud.  This requires some preparation:

  - Create a [Vagrant Cloud account](https://app.vagrantup.com/account/new).  Sign in.
    - Select _Create a new Vagrant Box_
    - Create a box named "ubuntu1804"
    - Create an access token.  Copy it to your clipboard as it will be displayed only once.
  - Create environment variable `VAGRANT_CLOUD_TOKEN` whose value is your access token:
    ```
    export VAGRANT_CLOUD_TOKEN=ab44...95ef
    ```

Change working directory to the directory containing the `ubuntu1804.json` file and run Packer, specifying your Vagrant Cloud username and a new version number for this Vagrant box:

    $ packer build -var "username=yourVagrantCloudUsername" -var 'version=1.2.0' ubuntu1804.json

After a few minutes, Packer should tell you the box was generated successfully, and the box was uploaded to Vagrant Cloud.

### Download and run

Change to a directory with no `Vagrantfile`:

    $ mkdir testing
    $ cd testing

Initialize Vagrant based on the box you uploaded to Vagrant Cloud:

    $ vagrant init yourVagrantCloudUsername/ubuntu1804

Start Vagrant:

    $ vagrant up

Open a terminal session with the Vagrant box.  The password is "vagrant":

    $ vagrant ssh

Observe how the host directory containing `ubuntu1804.json` is mounted on `/vagrant`:

    $ ls /vagrant

Exit the box:

    $ exit

Stop the box:

    $ vagrant halt

Delete the box and all associated files except `Vagrantfile`:

    $ vagrant destroy 

This concludes the procedure to create a Vagrant Box, upload it to Vagrant Cloud and run it.


## Usage without upload to Vagrant Cloud

### Build

If you do not care to push your box to Vagrant Cloud, edit the Packer configuration file `ubuntu1804.json`:

* Remove the `vagrant-cloud` post-processor from the end of the file
* Remove the user `variables` section at the beginning of the file, so that you need not set the variables on the command line

Then, change directory to the directory containing the `ubuntu1804.json` file and run Packer:

    $ packer build ubuntu1804.json

After a few minutes, Packer should tell you the box was generated successfully.

### Run

The included `Vagrantfile` allows quick testing of the built Vagrant boxes. From this same directory, run the following command after building the box:

    $ vagrant up

Open a terminal session with the Vagrant box:

    $ vagrant ssh

Observe  how the host directory containing `ubuntu1804.json` is mounted on `/vagrant`:

    $ ls /vagrant

Exit the box:

    $ exit

Stop the box:

    $ vagrant halt

Delete the box and all associated files except `Vagrantfile`:

    $ vagrant destroy 


## License

MIT license.

## Author Information

Created in 2018 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
