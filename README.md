# PodcasterWP

PodcasterWP uses [Trellis](https://github.com/roots/trellis), [Bedrock](https://github.com/roots/bedrock), and [Sage](https://github.com/roots/sage) as a starting point.

- Reproducible development environments with Vagrant
- Automate the configuration of high-performance production servers
- One-command deploys for your WordPress sites

Configure servers with a single command:

|                        | Command
| ---------------------- | ------------------------------------------------ |
| **Development**        | `vagrant up`                                     |
| **Staging/Production** |`ansible-playbook -i hosts/production server.yml` |
| **Deploying**          | `./deploy.sh production <site name>`             |

## What's included

* Ubuntu 14.04 Trusty LTS
* Nginx (with optional FastCGI micro-caching)
* PHP 5.6 (or [HHVM](http://hhvm.com/))
* [MariaDB](https://mariadb.org/) as a drop-in MySQL replacement (but better)
* SSL support (A+ on https://www.ssllabs.com/ssltest/)
* Composer
* WP-CLI
* sSMTP (mail delivery)
* Memcached
* Fail2ban
* ferm


## Requirements

* Ansible >= 1.9.2 (**not** 2.0 which is in alpha) - [Install](http://docs.ansible.com/intro_installation.html) • [Docs](http://docs.ansible.com/) • [Windows docs](https://roots.io/trellis/docs/windows/)
* Virtualbox >= 4.3.10 - [Install](https://www.virtualbox.org/wiki/Downloads)
* Vagrant >= 1.5.4 - [Install](http://www.vagrantup.com/downloads.html) • [Docs](https://docs.vagrantup.com/v2/)
* vagrant-bindfs >= 0.3.1 - [Install](https://github.com/gael-ian/vagrant-bindfs#installation) • [Docs](https://github.com/gael-ian/vagrant-bindfs) (Windows users may skip this)
* vagrant-hostsupdater - [Install](https://github.com/cogitatio/vagrant-hostsupdater#installation) • [Docs](https://github.com/cogitatio/vagrant-hostsupdater)

## Installation

1. Download/fork/clone this repo to your local machine.
2. Run `ansible-galaxy install -r requirements.yml` inside your Trellis directory to install external Ansible roles/packages.


## Development setup

1. Configure your [WordPress sites](#wordpress-sites) in `group_vars/development/wordpress_sites.yml`
2. Run `vagrant up`

## Remote server setup (staging/production)

For remote servers, you'll need to have a base Ubuntu 14.04 server already created.

1. Configure your [WordPress sites](#wordpress-sites) in `group_vars/<environment>/wordpress_sites.yml`. Also see the [Passwords docs](https://roots.io/trellis/docs/passwords/).
2. Add your server IP/hostnames to `hosts/<environment>`.
3. Specify public SSH keys for `users` in `group_vars/all/users.yml`. See the [SSH Keys docs](https://roots.io/trellis/docs/ssh-keys/).
4. Consider setting `sshd_permit_root_login: false` in `group_vars/all/security.yml`. See the [Security docs](https://roots.io/trellis/docs/security/).
5. Run `ansible-playbook -i hosts/<environment> server.yml`.

## Deploying to remote servers

Full documentation: https://roots.io/trellis/docs/deploys/

1. Add the `repo` (Git URL) of your Bedrock WordPress project in the corresponding `group_vars/<environment>/wordpress_sites.yml` file.
2. Set the `branch` you want to deploy (optional - defaults to `master`).
3. Run `./deploy.sh <environment> <site name>`
4. To rollback a deploy, run `ansible-playbook -i hosts/<environment> rollback.yml --extra-vars="site=<site name>"`

### Mail

sSMTP handles outgoing mail. For the `development` environment, emails are sent to MailHog, where you can inspect them. To access MailHog interface, go to `http://yourdevelopmentdomain.dev:8025`. For `staging` and `production`, configure credentials in `group_vars/all/mail.yml`. See the [Mail docs](https://roots.io/trellis/docs/mail/).

## Contributing

Contributions are welcome from everyone.
