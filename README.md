# Vagrantbox

This repo provides a template to create an Ubuntu-based development environment using the VirtualBox software hypervisor with orchestration by Ansible. It is primarily geared towards PHP and Rails development, but can support a range of dev needs. The box is configurable using YAML.


## The Stack
- Ubuntu 14.04 LTS
- Nginx
- MySQL
- PostgreSQL
- Redis
- PHP
- Ruby on Rails
- Puma
- Node
- NPM
- Composer


## Requirements

- [Vagrant](http://www.vagrantup.com/downloads.html)
- [Virtual Box](https://www.virtualbox.org/wiki/Downloads)
- [Ansible](http://docs.ansible.com/ansible/)


## TODO

Outstanding items:

- Provide configuration documentation
- Fix Puma automatic startup
- Implement switchable dev/production deployment
- Add Apache web server
- Enhance app configuration options


### Credits

The project was inspired by the excellent [Vagrant & Ansible Quickstart Tutorial](https://adamcod.es/2014/09/23/vagrant-ansible-quickstart-tutorial.html) by Adam Brett.

It is based on work done in the following projects:

- [Laravel Homestead](http://laravel.com/docs/5.1/homestead "Laravel Homestead")
- [railsbox](https://railsbox.io/ "railsbox")
- [phansible](http://phansible.com/ "phansible")
- [Scotch Box](https://box.scotch.io/ "Scotch Box")
