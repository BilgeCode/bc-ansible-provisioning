# Releasing BilgeCode using ansible

## Current Setup

  - Postgres Database with PostGIS support
  - Django Web App
  - Passage-Planner Javascript

### What is still needed

  - Caching

### Servers

  - 1 db server (postgres)
  - 1 app server (nginx, gunicorn, supervisor, memcached)
  - static media to s3 (passage-planner, static media) [maybe eventually]

### Questions:

  - best way to populate bower components?
    - run `bower install` after collectstatic?
    - what about vulcanize?
  - what performance hits would I take using oregon w/ zero co2 emissions?

Comments:

  - no need to create a worker server yet, I'll use cron to pull data


## Deployment Commands

### Vagrant

    $ vagrant up

And manually:

    $ vagrant provision <host>
    # or
    $ ansible-playbook \
      -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory \
      --private-key=~/.vagrant.d/insecure_private_key \
      -u vagrant provisioning/playbook.yml

## Reference

https://docs.djangoproject.com/en/dev/ref/contrib/gis/install/postgis/#spatialdb-template91

## Debugging tips

`vagrant provision app1` is your friend

## Cheatsheet

export DB_URL="postgis://bc:changeme@192.168.111.253/bc"
export STATIC_ROOT="/var/www/bilgecode/static/"
export MEDIA_ROOT="/var/www/bilgecode/media/"

  $ ansible-playbook -i linode_hosts playbook.yml -K -vvvv \
    --extra-vars "db_ip=192.155.94.37 root_domain_name=dev.bc.com dbpass=changeyou"

export DB_URL="postgis://bc:changeme@{{db_ip}}/bc"
export STATIC_ROOT="/var/www/bilgecode/static/"
export MEDIA_ROOT="/var/www/bilgecode/media/"
