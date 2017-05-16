# openio/sds Dockerfile

This image provides an easy way to run an OPENIO namespace with an Openstack Swift/Swift3 proxy.
It deploys and configure a simple non-replicated namespace in a single Docker container.

# Building master
$ docker build -t sds_source ubuntu/

# Building image with tag/branch
$ docker build -t sds_source:17.04a \
    --build-arg TAG_OIO_SDS=4.0.0.a3 \
    --build-arg TAG_SWIFT3=v1.11.0.4 \
    --build-arg TAG_SWIFT=stable/ocata \
    --build-arg TAG_OIO_SWIFT=1.0.0.b0 \
    ubuntu


# TODO

Align configuration with open-io/sds Docker image (at this time, Swift is exposed over port 5000 and not 6007)

## Documentation

To test with different object storage clients:
- [OpenIO SDS command line](http://docs.openio.io/user-guide/openiocli.html)
- [Openstack Swift command line (using TempAuth)](http://docs.openio.io/user-guide/swiftcli.html#tempauth)
- [AWS S3 command line](http://docs.openio.io/user-guide/awscli.html)
