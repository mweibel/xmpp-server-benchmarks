# XMPP Server Benchmarks

[![Build Status](https://travis-ci.org/mweibel/xmpp-server-benchmarks.png)](https://travis-ci.org/mweibel/xmpp-server-benchmarks)

This repository should evolve to a fully automated server benchmark suite which
tests various example scenarios using [tsung](https://github.com/processone/tsung).

It's currently in a very early stage, but if you'd like that, you're welcome to
contribute.

## Testing the setup

To see whether the playbook works is using [Vagrant](http://www.vagrantup.com)
and testing the automated provisioner.

To test using Vagrant, you need a working Virtualbox and Vagrant setup.
Then:

```
xmpp-server-benchmark$ cd provisioning
xmpp-server-benchmark/provisioning$ vagrant up
```

Repeated provisions you can run using:

```
xmpp-server-benchmark/provisioning$ vagrant provision
```

## Running on ec2

Make sure you have `boto` (`python-boto`) installed before you proceed.

You can either [create a file `~/.boto`](http://docs.pythonboto.org/en/latest/getting_started.html#configuring-boto-credentials)
for your AWS access/secret keys or specify it on the commandline using `AWS_ACCESS_KEY` and `AWS_SECRET_KEY`.

After that, you should be able to run the benchmarks using:

```
$> AWS_REGION=eu-west-1 ansible-playbook -i provisioning/ec2 provisioning/ec2.yml
```

Replace the region with your preferred AWS region.

The default configuration will provision and run the tests on 1 tsung / 1 xmpp server.
If you want to test with more, start the provisioning like this and replace the two counts
with want you'd like to have:

```
$> AWS_REGION=eu-west-1 ansible-playbook -i provisioning/ec2 --extra-vars "client_count=1 server_count=3" provisioning/ec2.yml
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
- [x] Automated collection of tsung results back from the testing server
- [x] EC2 setup
- [ ] Validate results are correct
- [ ] Automate publishing of reports on a GH-Page (maybe)
- [ ] Automated Prosody setup incl. config
- [ ] Automated Tigase setup incl. config
