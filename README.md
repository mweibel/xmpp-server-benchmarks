# XMPP Server Benchmarks

[![Build Status](https://travis-ci.org/mweibel/xmpp-server-benchmarks.png)](https://travis-ci.org/mweibel/xmpp-server-benchmarks)

This repository should evolve to a fully automated server benchmark suite which
tests various example scenarios using [tsung](https://github.com/processone/tsung).

It's currently in a very early stage, but if you'd like that, you're welcome to
contribute.

## Testing the setup

There's no way yet to run the benchmarks as thought on ec2 or wherever you'd like to.
The only way to see whether it works or not is using [Vagrant](http://www.vagrantup.com)
and testing the automated provisioner.

To test using Vagrant, you need a working Virtualbox and Vagrant setup.
Then:

```
xmpp-server-benchmark$ cd provisioning
xmpp-server-benchmark/provisioning$ vagrant up
```

## Contributing scenarios
In order to contribute a scenario you need to make three changes:

1. Add your scenario template as a [jinja2 template](http://jinja.pocoo.org/docs/dev/templates/) to the directory `provisioning/roles/scenarios/templates/`
2. Let the `scenarios` role copy your template on provisioning by adding an entry to `provisioning/roles/scenarios/tasks/main.yml`
3. Add your scenario to the `enabled_scenarios` list in `provisioning/group_vars/all`

## Contributing servers to test
Please have a look at an existing server setup in order to see how to do it.

1. Take a look into the `ejabberd` role (`provisioning/roles/ejabberd`)
2. Add your own role (or roles) (possibly by reusing one from [ansible galaxy](https://galaxy.ansible.com))
3. Add the new role to `provisioning/setup.yml`
4. Configure the server as enabled in `provisioning/group_vars/all` similar to existing entries

## ToDo

- [x] Basic ansible & vagrant setup
- [x] Automated ejabberd setup incl. config
- [x] Automated MongooseIM setup incl. config
- [x] Basic tsung scenario for testing
- [x] Automated start of configured servers based on what to test & test them
- [ ] Automated collection of tsung results back from the testing server
- [ ] EC2 setup
- [ ] Automated Prosody setup incl. config
- [ ] Automated Tigase setup incl. config

## How I imagine the testing should work

- 1-n nodes of tsung doing the load testing
- 1-n nodes of the xmpp servers being tested

On the xmpp server nodes each server software is installed and then started one by
one after a tsung scenario has been completed.