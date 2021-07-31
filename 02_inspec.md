# Chef Inspec

<https://community.chef.io/tools/chef-inspec>  

Turn your compliance, security, and other policy requirements into automated tests.

```bash
curl https://omnitruck.chef.io/install.sh | sudo bash -s -- -P inspec
git clone https://github.com/dev-sec/cis-docker-benchmark.git
inspec exec cis-docker-benchmark/ # Test of the docker platform
```
