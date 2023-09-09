# Setup for vdirsyncer
vdirsyncer will sync the read-only monica calendar to an external calendar.

## docker-compose.yml
```
  vdirsyncer:
    image: bleala/vdirsyncer:latest
    container_name: vdirsyncer
    restart: unless-stopped
    environment:
      - TZ=America/Chicago
      - AUTOSYNC=true
    volumes:
      - /srv/docker/vdirsyncer/config:/vdirsyncer/config
```

## /srv/docker/vdirsyncer/config
```
[general]
status_path = "/vdirsyncer/status/"

[pair monica_mailcow]
a = "monica_birthday"
b = "remote_birthday"
collections = [["mybirthdays","birthdays", "*** remote calendar id ***"]]
conflict_resolution = "a wins"
partial_sync = "ignore"

[storage monica_birthday]
type = "caldav"
url = "https://monica_host/dav/calendars/*** monica username ***/"
username = "*** monica username ***"
password = "*** api key here ***
read_only = true

[storage remote_birthday]
type = "caldav"
url = "*** cal dav url ***"
username = "*** remote username ***"
password = "*** remote password or app specific password ***"
```

Run the following commands:
`docker-compose up -d`
`docker exec -it vdirsyncer vdirsyncer discover`
`docker exec -it vdirsyncer /bin/bash -c "vdirsyncer metasync && vdirsyncer sync"`

### Refrences
https://github.com/Bleala/Vdirsyncer-DOCKERIZED/tree/main
https://vdirsyncer.pimutils.org/en/stable/tutorial.html
