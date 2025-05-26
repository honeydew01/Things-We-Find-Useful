# SSH Keys

A short tutorial on how to set up ssh keys for using Github

## Generating an SSH Key

To generate a key, run this command in a bash terminal: ```ssh-keygen```. It should select the most up to date encryption standard to use.

When prompted for the file name, enter the path of the key file (best to put in the .ssh folder in the home directory) and name the file so that you know which github account the key is linked to.

Then open the public key file and copy the contents.

## Adding the SSH Key to Github

Once you have generated a public-private key pair, open github and navigate to the settings under "SSH Keys". Add the contents of the public ssh key to the ssh key section and name the key so you know which device owns the private key.

## Configuring Git

Now that you have a public and private key pair, you need to configure your local git. There are two ways you can do this. The first option is modifying the global or local .gitconfig file manually. The second option is to use git commands to modify the .gitconfig file.

### Modifying .gitconfig Manually

To modify the global git settings, open the .gitconfig in the home directory (If you want to do it for a local directory, open the .git directory and there should be a config file inside).

In the config file, there should be a "core" section. If not, add ```[core]``` section in the config file (aligned left). Under the core section (indented by one), add the following line: ```sshCommand = ssh -i {/path/to/private/key/file}```

Also make sure that the user section is correctly configured with your email and username. You can do so by adding a ```[user]``` section with the fields, ```name = ...``` and ```email = ...```

Now git should be configured correctly to use ssh keys!

### Using Git Commands to Configure Git

You may also modify the configuration of git using git commands:

#### Adding the SSH Command

```git config --{global/local} core.sshCommand "ssh -i {/path/to/private/key/file}"```

#### Modifying User Fields

```git config --{global/local} user.name "..."```

```git config --{global/local} user.email "..."```

For each of these commands, you may choose to configure the local directory or the global git setting.
