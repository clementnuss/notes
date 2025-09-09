# Shrinking container images (Thomas Fricke)

effective security measures:

- remove serviceaccount token if not needed

examples of security breaches:

- log4j
- imagetragick-poc: <https://github.com/thomasfricke/training-kubernetes-security/blob/main/OpenShift/ImageTragickBuild.ipynb>

problems with bloated container images:

- blinds security (as there are so many CVEs)
- curl/python are not needed for a tomcat, obviously

## hardening

hardening at docker build time: use the `harden` script to copy only the files
which are needed: you create a new container

### BLAFS: container debloating

tool which creates a shadow file system on top of the original image, where only
needed runtime files are present: <https://github.com/negativa-ai/BLAFS>

debloat result: 60 to 95% reduction in image size
