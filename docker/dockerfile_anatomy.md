**FROM:** Docker images are layered, each command creates a new layer, by using the FROM directive you specify a base image upon which to build.

**MAINTAINER:** simply the name (and most of the times the email) of the maintainer.

**ADD:** the add directive copies the files from a source destination inside the container at build time.

**RUN:** the run directive runs a command at build time.

**CMD:** like the run directive, but CMD doesn’t run at build time. Instead it runs at run time, when the container is created.

**ENV:** this directive is one of the most used, it allows to define local variables just as you would when executing a script.

**EXPOSE:** the expose directive is much like a firewall port, it tells that the container “listens” to that port.

**WORKDIR:** the active directory where commands will operate. Think of this directory as the “cd” command inside the container to help you visualize what it does.

**VOLUME:** volumes are a way for containers to store persistent data, if you don’t know about them I suggest you read here.

**ENTRYPOINT:** this directive is the core of a Dockerfile, it is the process that is started when the container is run.
