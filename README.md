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

## ToDo

- [x] Basic ansible & vagrant setup
- [ ] Automated ejabberd setup incl. config
- [ ] Automated MongooseIM setup incl. config
- [ ] Basic tsung scenario for testing
- [ ] Automated collection of tsung results and storing them in the repo (most likely)
- [ ] EC2 setup
- [ ] Automated Prosody setup incl. config
- [ ] Automated Tigase setup incl. config

## How I imagine the testing should work

- 1-n nodes of tsung doing the load testing
- 1-n nodes of the xmpp servers being tested

On the xmpp server nodes each server software is installed and then started one by
one after a tsung scenario has been completed.