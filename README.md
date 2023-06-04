# Requirements
- Install [Vagrant](https://developer.hashicorp.com/vagrant/docs/installation).
- Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
- Make sure to have enough space on your default drive. Go to [Troubleshooting](#troubleshooting) for changing the default location where Vagrant and VirtualBox store its data.

# Run
- Clone this repository:
  ```
  git clone https://github.com/ed3899/vagrant-base-ubuntu-jammy <MY_PERSONAL_VM>
  ```

- Create a local branch:
  ```
  git branch -b <MY_LOCAL_BRANCH>
  ```

- On the `Vagrantfile`, select your tags by uncommenting them (some tags may require additional setup, go to [Tags](#tags)):
  ```
      ansible.tags = [
        #? Cloud providers
        # "aws",
        #? Container tools
        # "docker",
        # "k8s_tools",
        # "minikube",
        #? Dbs
        # "mysql",
        # "pg-15",
        #? Programming languages
        # "dotnet_sdk",
        # "go",
        # "node_js",
        # "python_anaconda",
        # "rust",
        #? Terminal
        # "starship",
        #? UI
        # "gui",
        #? Vc
        # "git",
        # "github",
      ]
  ```

Change that according to the tags you wish to run. Each tag maps to an ansible play. The order you pick doesn't matter.

- And run:
  ```
  vagrant up
  ```

  If you ran into issues while downloading the box, go to [Troubleshooting](#firewall-issues-while-downloading-the-box)

- Once changes are done, ssh into the VM:
  ```
  vagrant ssh
  ```

- If you've picked a new tag after the initial provisioning, run:
  ```
  vagrant provision
  ```

- If for some reason you lose you ssh connection, run:
  ```
  vagrant reload
  ```
  and try sshing again.

- If you want to reboot the machine and provision again, run:
  ```
  vagrant reload --provision
  ```
  and try sshing again.

# Share folders
On the `Vagrantfile`, make sure you shared the additionals folders you would like to have
in sync with the VM with the following line:


```
config.vm.synced_folder "./HOST/FOLDER", "/GUEST/FOLDER", owner: "vagrant", group: "vagrant", create: true
```

By default the `ansible` and `assets` folders are shared.

The first one is needed for provisioning while the latter is for resources you would like to share with your VM, place those in there anytime, no need to reload (i.e images, csv, etc).

This is useful if you decide to SSH via VS Code Remote SSH Extension or another method (see [Using with VS Code Remote SSH Extension](#using-with-vs-code-remote-ssh-extension))

Be mindful of sharing folders with initialized git repositories. If you decide to fork and make changes to this repository some of those may conflict with children repos.

Add those to the `.gitignore` file of this repo to avoid conflicting repo issues with the children.

Or you can set them as git sub-modules.

Once done, reload the vm for the changes to take effect:

```
vagrant reload
```

# Tags
## Cloud providers
### AWS
If using the `aws` tag.

Create or fill:

`ansible\playbooks\secrets\main.yml`

With values:

```
aws:
  access_key_id: YOUR_KEY
  secret_access_key: YOUR_SECRET_KEY
  region: us-west-2
  output: json
```

This file is ignored by git

## Programming Languages

### Node.js / Javascript
Uncomment the `node_js` tag.

It uses [nvm](https://github.com/nvm-sh/nvm) to manage node versions.

### Python
Uncomment the `python_anaconda` tag.

The python distro used is [conda](https://docs.conda.io/projects/conda/en/latest/index.html)

Conda manages dependencies and virtual environments.

## Databases
## Postgres
All postgreSQL tags have the following available users and database:

```
user: postgres
password: postgres
database: vagrant
```

If you want to interact with your database from a VSCode extension, make sure you've got a [Private Network](https://developer.hashicorp.com/vagrant/docs/networking/private_network) on Vagrant between the host and the guest.

## Version control

## Git
If using the `git` tag

Populate `vars/main.yml` with your git username and email.

```
git:
  username: YOUR_GIT_USERNAME
  email: YOUR_EMAIL_FOR_GIT

```

## GitHub
If using the `github` tag.

Create or fill:

`ansible/playbooks/secrets/main.yml`

With values:

```
github:
  tokens:
    personal_access_token_classic: YOUR_TOKEN
```

This file is ignored by git

Make sure you remove your old keys from GitHub when you destroy your VMs. This is done automatically once you remove your expired personal access tokens.

Make sure to give the right permissions to the token as well (i.e "repo", "admin:public_key" are the minimum). For more granular control consult the docs.

[GitHub_Personal_Access_Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

# Skip Tags

## Always, Init
Comment these out if you would like to test quick changes without running base setup again.

Or feel free to place here tags you have already ran.

## Using with VS Code Remote SSH Extension
Run `vagrant ssh-config > some-file.txt`. This will generate a file with the configuration to run using SSH. Here an example of that file:

```
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile C:/Users/User/project/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
  ForwardAgent yes
  ForwardX11 yes
```

Notice that the `HostName` is default, you could rename it to whatever you want so you could identify it more easily.

Copy the content of `some-file.txt` inside your SSH configuration file. This file could be edit directly from vscode by pressing F1 and writing Remote-SSH: Open Configuration File..., then you select the file you use for ssh configuration. After that file opens, just copy the content of some-file.txt there.

Finally, just press again F1 and type Remote-SSH: Connect to Host..., choose the connection with the `HostName` default or the want you wrote in the first step, and that's all.

You should be able to ssh into the machine.

[Stack OverFlow Source](https://stackoverflow.com/a/62200336/11941146)

## Troubleshooting

### Firewall issues while downloading the box
Please refer to this [thread](https://stackoverflow.com/questions/72290594/unknown-error-0x80092012-trying-to-configure-vagrant-with-git-bash/75837342#75837342) where it is explained in detail.

If you want a quick and dirty fix, disable your firewall or antivirus.

### Change location where Vagrant stores boxes on Windows
If you want to change the default location where Vagrant stores
the boxes . Run the following on CMD or Powershell. By default is stores them
on `C:\Users\USERNAME\.vagrant.d\boxes`

`Set-Item Env:VAGRANT_HOME 'D:\Example\Location'`

This will only change the environment variable per terminal session.

If you wish to change it system wide then go to:

*Home > Edit the System Environment Variables (admin password prompt required) > Environment Variables > System Variables > New*

Then set the values accordingly:

```
Variable name: VAGRANT_HOME
Variable value: D:\Example\Location
```

Reboot your host machine

This is important if you have limited space on your default hard drive.

### Change location where VirtualBox stores VMs on Windows
If you're using VirtualBox as a provider make sure you change
the default location where it stores the VMs.

**File > Preferences**

Select the dropdown **Default Machine Folder** and choose your
custom location

This is important if you have limited space on your default hard drive.

### Cannot detect my Python environments in VS Code once connected with the Remote SSH extension
Look for you remote SSH `settings.json`

Add the following:

```
{
  "python.venvFolders": ["/anaconda3/envs"],
  "python.condaPath": "/anaconda3/condabin/conda"
}
```