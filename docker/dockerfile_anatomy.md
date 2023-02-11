**FROM:** Specify a base image upon which to build.

**ADD:** Copies the files from a source destination inside the container at ***build time***.

**ENV:** Define local variables just as you would when executing a script.

**EXPOSE:** Tells that the container listens to that port.

**WORKDIR:** "cd" inside the container.

**VOLUME:** volumes are a way for containers to store persistent data.

**RUN:** Runs a command at ***build time*** using a base image that does not contain the specified shell executable. This form `RUN ["<executable>", "<param1>", "<param2>"]` makes it possible to avoid shell string munging

**CMD:** It runs at the ***run time***, there can only be one CMD instruction in a Dockerfile to provide defaults for an executing container. These defaults can include an executable, or they can omit the executable, in which case you must specify an **ENTRYPOINT** instruction as well. `CMD ["<executable>", "<param1>", "<param2>"]`

**ENTRYPOINT:** it is the process that is started when the container is run.  `ENTRYPOINT ["<executable>", "<param1>", "<param2>"]`


COPY

LABEL

STOPSIGNAL

USER

VOLUME

ONBUILD

# RUN vs CMD vs Entrypoint

- The `RUN` command always gets executed in a new layer. It allows you to install packages and applications on top of an existing image layer and creates a new layer on top of it.
-  `CMD` used to set a default command. This will be executed if you run a particular container without specifying some command. In case you specify a command while running a docker container, the default one will be ignored. Note that if you specify more than one `CMD` instruction in your dockerfile, only the last one will be executed. Also, it could be used to set some additional default parameters that will be inserted after the default parameters when you are using an `ENTRYPOINT` in executable form and if you run the container without specifying any arguments in the command line.
- `ENTRYPOINT` is similar to a `CMD` command. However, it does not ignore the parameters when you run a container with CLI parameters. 

If you want to specify default arguments and want it to be overwritten on specifying CLI arguments, use `CMD` commands. And if you want to run a container with the condition that a particular command is always executed, use `ENTRYPOINT`. `RUN` is simply used to build additional image layers over the base image.


## Executable form
```shell
RUN ["apt-get", "install", "vim"]
ENTRYPOINT ["/bin/echo", "TutorialsPoint"]
CMD ["/bin/echo", "TutorialsPoint"]
```

## Shell form
```shell
RUN apt-get -y update
CMD echo "TutorialsPoint"
ENTRYPOINT echo "TutorialsPoint"
```
