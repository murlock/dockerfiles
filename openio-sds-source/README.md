# openio/sds Dockerfile

This image provides an easy way to run an OPENIO namespace with an Openstack Swift/Swift3 proxy.
It deploys and configure a simple non-replicated namespace in a single Docker container.

# TODO

Align configuration with open-io/sds Docker image (at this time, Swift is exposed over port 5000 and not 6007)

## Documentation

To test with different object storage clients:
- [OpenIO SDS command line](http://docs.openio.io/user-guide/openiocli.html)
- [Openstack Swift command line (using TempAuth)](http://docs.openio.io/user-guide/swiftcli.html#tempauth)
- [AWS S3 command line](http://docs.openio.io/user-guide/awscli.html)
