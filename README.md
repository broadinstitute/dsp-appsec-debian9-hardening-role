debian9-CIS Ansible Role
=========

This role can be used in an Ansible playbook to harden Debian 9 images to CIS 1.0.1 standards.

![Molecule Test](https://github.com/broadinstitute/dsp-appsec-debian9-hardening-role/workflows/Molecule%20Test/badge.svg)

Requirements
------------

None

Role Variables
--------------

None

Dependencies
------------

None

Example Playbook
----------------

This role is currently not available on Ansible Galaxy, and must be included locally in the project. Please see `Set Up` for more information.

    - name: Harden Server
    hosts: localhost
    become: yes

    roles:
    - dsp-appsec-debian9-hardening-role
    
Set Up
------

1. Make sure vagrant and virtualbox are installed.
```
brew cask install vagrant virtualbox
```

2. Create project folder and change into it before initializing the vagrant project with a Debiam9 base image. This should genertate a `Vagrantfile` and a `.vagrant` directory. You can then test whether the Vagrant project was created by running vagrant up to start the machine.

```
# Create project folder
mkdir -p ~/MyProjects/deb9-test-box
cd ~/MyProjects/deb9-test-box

# Initialize vagrant 
vagrant init debian/stretch64

# Check if VM image was created
vagrant up
vagrant ssh -c "ip a"
vagrant destroy
```
3. Make sure ansible is installed.
```
brew install ansible
```
4. Create an ansible playbook, containing the yaml below.
```
- name: Harden Server
  hosts: all
  become: yes

  roles:
  - debian9-CIS
```
5. Create a `roles` folder within the the project and change into it. Clone this repo into the folder.
```
mkdir roles
cd roles
git clone https://github.com/broadinstitute/debian9-CIS.git
```
6. Open your Vangrant file and look for the `config.vm.box = "debian/stretch64"` line. Edit the Vagrantfile so it looks like the yaml below.
```
...
  config.vm.box = "debian/stretch64"
  
  # Add the following code
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "[YOUR-ANSIBLE-PLAYBOOK].yml"
  end
...
```
7. To run the role, run `vagrant up`. The ansible playbook will automatically run when VM boots. While the VM is running, you can run `vagrant provision` to re-run the playbook.



License
-------

BSD

Author Information
------------------

Please contact `appsec@broadinstitute.org` with any questions.
