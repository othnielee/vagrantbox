# Vagrantbox

This repo provides a template to create an Ubuntu-based development environment using the VirtualBox software hypervisor, with orchestration by Ansible. It's geared towards building Laravel and Rails apps, but can be used for other dev projects. Multiple apps / projects are supported on a single box, and these are configurable using YAML.


## The Stack
- Ubuntu 16.04 LTS
- Nginx
- MySQL
- PostgreSQL
- Redis
- PHP 7.0
- Ruby on Rails
- Puma
- Node
- NPM
- Composer


### Requirements

- [Vagrant](https://www.vagrantup.com/downloads.html)
- [Virtual Box](https://www.virtualbox.org/wiki/Downloads)
- [Ansible](http://docs.ansible.com/ansible/)


### TODO

Outstanding items:

- Configuration documentation
- Fix Puma automatic startup
- Implement switchable dev/production deployment
- Enhance app configuration options


### Credits

The project was inspired by the excellent [Vagrant & Ansible Quickstart Tutorial](https://adamcod.es/2014/09/23/vagrant-ansible-quickstart-tutorial.html) by Adam Brett.

It is based on work done in the following projects:

- [Laravel Homestead](https://laravel.com/docs/5.3/Homestead "Laravel Homestead")
- [railsbox](https://railsbox.io/ "railsbox")
- [phansible](http://phansible.com/ "phansible")
- [Scotch Box](https://box.scotch.io/ "Scotch Box")
