**FROM:** Specify a base image upon which to build.

**ADD:** Copies the files from a source destination inside the container at ***build time***.

**RUN:** Runs a command at ***build time*** using a base image that does not contain the specified shell executable. This form `RUN ["<executable>", "<param1>", "<param2>"]` makes it possible to avoid shell string munging

**CMD:** It runs at the ***run time***, there can only be one CMD instruction in a Dockerfile to provide defaults for an executing container. These defaults can include an executable, or they can omit the executable, in which case you must specify an **ENTRYPOINT** instruction as well.

**ENV:** Define local variables just as you would when executing a script.

**EXPOSE:** Tells that the container listens to that port.

**WORKDIR:** "cd" inside the container.

**VOLUME:** volumes are a way for containers to store persistent data.

**ENTRYPOINT:** it is the process that is started when the container is run.


COPY

LABEL

STOPSIGNAL

USER

VOLUME

ONBUILD
