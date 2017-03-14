Alpine glibc
===
A very small Alpine linux base image for programs that require glibc. Total size is 9.914MB, one of the smallest containers to feature a fully functional glibc implementation!

## Usage
On its own, this dockerfile does not do anything. However it is used as a base for programs that require glibc, so they can be run on Alpine Linux. This is achieved by installing the libc6, libstdc++6 and libgcc1 libraries alongside the existing musl versions of these libraries. These can be found in ```/glibc/lib``` inside the container.

To make programs use the glibc libraries instead of the musl ones, patchelf must be used to point the binaries at the correct (glibc) interpreter. This is included by default in this image, and can be used as so:
```patchelf --set-interpreter $GLIBC_LD_LINUX_SO <binary>``` .

It is recommended that any programs that require glibc are placed in the ```/glibc``` folder, as this increases the odds that the program will detect and load the glibc libraries, rather than the built in musl libraries.

For an example implementation of this container see my [Plex Media server container](https://github.com/Adam-Ant/alpine-plexmediaserver) ([adamant/alpine-plex on Docker hub](https://hub.docker.com/r/adamant/alpine-plex/))

## Environment Variables
To make building applications using this container easier, it comes with some environment varaibles pre-configured. These are:
* **$DESTDIR** - this maps to ```/glibc``` inside the container. Useful for copying programs into the correct location to detect the glibc libraries.
* **$GLIBC_LIBRARY_PATH** - This is a standard glibc environment varaible used to locate the glibc libraries. Can also be used to add non-standard libraries to the correct place.
* **$GLIBC_LD_LINUX_SO** - Another standard glibc environment variable, mapped to ```/glibc/lib/ld-linux-x86-64.so.2``` by default.
