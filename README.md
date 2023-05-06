# dockrsync

Simple tool to help rsync docker volumes with another host


## Initial setup

* Create an rsync image on your docker host(s) by cloning this
  repo onto your docker host(s) and running mkimage in the
  rsync-image folder.

* Clone this repo to somewhere on any hosts that need access
  to the volumes on your docker host(s).


## Usage

Now, you can rsync anything in any volume on any docker host by using the
"dockervolume" helper script as the ssh shell, like so:
```
rsync -a --rsh=./dockervolume volumename@dockerhost:/mnt/ local/folder
```

This command will mount `volumename` at `/mnt` in a temporary container on
`dockerhost` and copy everything to the local folder `local/folder`.

This should work for any volume on any docker host that you have access to via
SSH.


## Limitations

* The remote path must always start with "/mnt/", as this corresponds to the
  mount point in the temporary docker container where the volume is mounted.

* This script assumes you'll be connecting to the docker hosts as the same
  user, or that the user is configured in your SSH config; this is because the
  username is hijacked

* No testing has been done with paths or other arguments that contain spaces;
  most likely, it will break;


## Other ideas

To restore support for specifying a user for the remote end, we could allow
syntax such as user@volume@host: and parse user and volume from the user that's
supplied to the helper script.

We could make dockrsync a command and have it handle the dockervolume helper.
