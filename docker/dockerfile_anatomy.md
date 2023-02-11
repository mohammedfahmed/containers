**FROM:** Specify a base image upon which to build.

**ADD:** Copies the files from a source destination inside the container at ***build time***.

**RUN:** Runs a command at ***build time***.  'RUN ["<executable>", "<param1>", "<param2>"]'

**CMD:** like the run directive, but CMD doesn’t run at build time. Instead it runs at run time, when the container is created.

**ENV:** Define local variables just as you would when executing a script.

**EXPOSE:** Tells that the container listens to that port.

**WORKDIR:** “cd” inside the container.

**VOLUME:** volumes are a way for containers to store persistent data.

**ENTRYPOINT:** it is the process that is started when the container is run.


COPY
LABEL
STOPSIGNAL
USER
VOLUME
ONBUILD
