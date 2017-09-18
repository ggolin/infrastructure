# infrastructure

[![](https://img.shields.io/badge/made%20by-Protocol%20Labs-blue.svg?style=flat-square)](http://ipn.io)
[![](https://img.shields.io/badge/project-IPFS-blue.svg?style=flat-square)](http://ipfs.io/)
[![](https://img.shields.io/badge/freenode-%23ipfs-blue.svg?style=flat-square)](http://webchat.freenode.net/?channels=%23ipfs)
[![standard-readme compliant](https://img.shields.io/badge/standard--readme-OK-green.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)

> Tools for maintaining infrastructure for the IPFS community.

- Overview
- Getting started
- Usage
- Known issues
- Common tasks


### Overview

This repository contains the technical infrastructure of the IPFS community.

- Public HTTP-to-IPFS Gateway: https://ipfs.io
- Default bootstrap nodes used by IPFS: `ipfs bootstrap`
- Private networking between the hosts
- Monitoring of services and hosts: http://metrics.ipfs.team
- Pinbot, an IRC bot in chat.freenode.net/#ipfs-pinbot
- Seeding of $important objects, in `seeding/` (not really maintained at the moment)
  - This is obsolete and kept purely because nobody has made sure yet that

Infrastructure that isn't contained here:

- Websites deployment: done in each repo separately, e.g. ipfs/website, ipfs/blog, libp2p/website, ipfs/distributions
- DNS: done pretty much manually through DNSimple dashboard -- and ipfs.io and ipfs.team are still in DigitalOcean
- CI: https://github.com/ipfs/jenkins


### Getting started

We use a custom tool called Provsn to maintain the setup of hosts and services.
The fundamental principle of Provsn is that hosts are in a certain state,
and units of code are run to transition into a different state.

Provsn is a plain shell script, and each unit consists of shell scripts too:
- The `env` script exposes variables and functions to the unit itself, and other units.
- The `build` script is run on the client and builds container images, config files, etc.
- The `install` script is run on the host and transitions it into the desired state.

**Note:** there are a few bits of Ansible code left over, which are to be migrated to Provsn.
You can find them in the `ansible/` directory.

To test whether you're all set up, execute a simple command on all hosts.

```sh
> ./provsn exec all 'whoami'
pluto: root
uranus: root
[...]
```

Two environment variables can be used to alter Provsn's operation:

- `PROVSN_JOBS` -- this controls the number of hosts to run on in parallel, and defaults to 4.
- `PROVSN_TRACE` -- if set, this enables Bash tracing (`set -x`) for extensive debugging information.
  Note that this *will contain sensitive information and secrets*.

### Usage

### Known issues

- no verbose option, need to comment out dev-null-redirections in unit scripts

### Common tasks

- gathering ipfs debug info
- updating ipfs
- deploying a website
- adding a root user
- adding hashes to the blocklist

### How can I get ssh access to the instances?

Add you ssh-key to the list of keys available in `base/env.sh`, like this: https://github.com/ipfs/infrastructure/blob/master/base/env.sh#L9 and then submit a PR with the changes.

### Other community infrastructure:

More info in https://github.com/ipfs/community

- Github
  - https://github.com/ipfs
  - https://github.com/libp2p
  - https://github.com/ipld
  - https://github.com/multiformats
  - https://github.com/protocol
  - https://github.com/ipfsbot
- Communication
  - IRC: chat.freenode.net/#ipfs and https://chat.ipfs.io
  - ipfs-users group: https://groups.google.com/forum/#!forum/ipfs-users
  - Slack
  - https://twitter.com/ipfsbot
- CI / Testing
  - GitCop
  - TeamCity: http://ci.ipfs.team:8111
  - Travis CI
  - Circle CI

## Contribute

Feel free to join in. All welcome. Open an [issue](https://github.com/ipfs/infrastructure/issues)!

This repository falls under the IPFS [Code of Conduct](https://github.com/ipfs/community/blob/master/code-of-conduct.md).

### Want to hack on IPFS?

[![](https://cdn.rawgit.com/jbenet/contribute-ipfs-gif/master/img/contribute.gif)](https://github.com/ipfs/community/blob/master/contributing.md)

## License

MIT
