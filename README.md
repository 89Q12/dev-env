#  Automated dev env using Ansible and Renovate

I got sick of having snowflakes installations around me all time
and so I first thought how can I share my dot-files in a sensible way?
I also know that I need the same setup everywhere at all time and synced data, so I need a way of doing that and what better way is there to have your own cloud dev-env on your own server, gotcha is that it only works when you're online. 
Hence both offline(local tools) and cloud(same tools except GUI ones) envs in one solution.

Ah! A dot-file repo, hmm I know ansible!
Let's do that and automate the setup of every machine I own!

Thats the gist of it :)
# Roadmap

- [x] Initial planning and repo
- [ ] finish install script 
- [ ] write required roles
- [ ] add cloud functionality
    - [ ] Server side things
        - [ ] Add playbook that setups 
            - wireguard
            - samba shares and makes those accessible on the wg0 interface
            - clones all git repos from the configured remote use into a specific folder structure
    - [ ] Client side
        - [ ] Script to check for internet/vpn connectivity
        - [ ] Login script: auto mount samba shares
        - [ ] Auto SSH script loaded via bashrc
            - [ ] Sync back git changes after SSH session exits when internet connectivity is there
        - [ ] Adjust add role that configures the client
- [ ] Repo sugar like Renovate

## Can I use this?

Yes! You have to customize the roles to your liking though!

## But I don't see any variables like the ones all the work roles use

While I have most variables store via ansible-vault, I can't do this for the work stuff, the roles and ansible expect a vault file in ```$HOME/.ansible-vault/```.
## How it works

The script in ```bin/```  installs the prerequisites before it executes the ansible playbook that then on first run installs all required roles and then installs either the private, work or cloud roles, depending on the machine type.
This works via tags that you give the initial script as arguments.
All sensitive variables are encrypted via ansible-vault, the secret for that is expected at ```$HOME/.ansible-vault/vault.secret```

## Cloud dev-env

A script on login(basically boot) checks that your machine is online and waits to be connected to the VPN(should essentially happen after login on mac or network-online.target on systemd machines has been reached).
Once that happened it mounts a samba share from the server*.
Another script loaded via bashrc checks for internet and auto SSHs into the server*, once you close the SSH session or it dies, it checks again for internet and if that is there tries to sync all git repos locally(This is to allow offline functionality)
When offline well you're offline and above things won't happen

Server: any linux host reachable via the VPN, that also is the VPN server and samba server.