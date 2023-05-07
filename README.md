# dockrsync

Simple tool to help rsync files with docker volumes on another host


## Why?

Docker manages persistent storage as volumes, but the volumes are only
"properly" accessible within a docker container.  In theory you could peek
into the underlying backing store, but this feels "dirty" somehow.

The usual way to access files in a docker volume seems to be to create a
temporary container with the volume mounted in it, and access or manipulate the
files within that container.  This is fairly simple and straightforward from
the docker host itself, but what if you want to access the docker volume(s)
from another host, for backup or other purposes?

Enter dockrsync: a simple mechanism that allows you to use rsync to access any
docker volume on your docker hosts.


## Initial setup

* Create an rsync image on your docker host(s) by cloning this
  repo onto your docker host(s) and running mkimage in the
  rsync-image folder.

* Clone this repo to somewhere on any hosts that need access
  to the volumes on your docker host(s), and place dockervolume
  in your path on those hosts.


## Usage

Now, you can rsync anything in any volume on any docker host by using the
"dockervolume" helper script as the ssh shell, like so:
```
rsync -a --rsh=dockervolume dockerhost:/mnt/volumename/ local/folder
```

This command will mount `volumename` at `/mnt/volumename` in a temporary
container on `dockerhost` and copy everything to the local folder
`local/folder`.

This should work for any volume on any docker host that you have access to via
SSH.

Note that rsync works in both directions: you can copy files to or from your
docker volumes, just like you can with regular hosts.


## Limitations

* The remote path must always start with "/mnt/" and a volume name and "/", as
  this corresponds to the mount point in the temporary docker container where
  the volume is mounted and specifies the docker volume to mount.

* No testing has been done with paths or other arguments that contain spaces;
  most likely, it will break;


## Other ideas

We could make dockrsync a command and have it handle the dockervolume helper.
