docker-openstack-stress
=======================

A simple docker container which provides the linux stress tool. Stress you system!


This container is the openstack variant of https://github.com/chilicat/docker-stress
It uses the metadata service to resolve the arguments for the stress command.

A docker container for openstack which provides the stress tool. It uses cloud-init to resolve the arguments to execute [stress](http://linux.die.net/man/1/stress)

Use heat to start instances, heat-stress.yaml:

    HeatTemplateFormatVersion: '2012-12-12'
    Parameters: {}
    Mappings: {}
    Resources:
      stress:
        Type: OS::Nova::Server
        Properties:
          flavor: m1.tiny
          image: stress:latest
          user_data:
            ARGS="--cpu 8 --io 4 --vm 2 --vm-bytes 128M"

The ARGS can contain any arguments supported by [stress](http://linux.die.net/man/1/stress)

And start the the stack:

    heat stack-create -f heat-stress.yaml stress

Take a look at the processor usage:

     9504 root      20   0  6560   88    0 R   26  0.0   0:04.08 stress
     9510 root      20   0  6560   88    0 R   26  0.0   0:03.97 stress
     9506 root      20   0  6560   88    0 R   24  0.0   0:03.89 stress
     ...
