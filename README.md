#  Automated dev env using Ansible and Renovate

I got sick of having snowflakes installations around my all time
and so I first thought how can I share my dotfiles in a sensbile way? 

Ah! A dotfile repo, hmm I know ansible!
Let's do that and automate the setup of every machine I own!

Thats the gist of it :)

## Can I use this?

Yes! You have to customize the roles to your liking though!

## But I don't see any variables like the ones all the work roles use

While I have most variables store via ansible-vault, I can't do this for the work stuff, the roles and ansible expect a vault file in ```$HOME/.ansible-vault/```.
## How it works

The script in ```bin/```  install the prerequisits before it execute the ansible playbook that then first install all required roles and then install either the private or the work roles, depending on the machine type.
This works via tags that you give the initial script as arguments.
All sensitive variables are encrypted via ansible-vault, the secret for that is expected at ```$HOME/.ansible-vault/vault.secret```
