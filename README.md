# thoschworks/cups-airprint-bjnp

Fork from [chuckcharlie/cups-avahi-airprint](https://github.com/chuckcharlie/cups-avahi-airprint)

This "reUbuntunized" Docker image runs a CUPS instance that is meant as an AirPrint relay for printers that are already on the network but not AirPrint capable. I forked [_chuckcharlie's_ project](https://github.com/chuckcharlie/cups-avahi-airprint) because I need the support for Canon printers using the proprietary USB over IP BJNP protocol. I switches back to _Ubuntu_ as base OS because I was not able to add the package [`cups-backend-bjnp`](https://launchpad.net/ubuntu/+source/cups-bjnp) to the Alpine version.

## Configuration

### Volumes:
* `/config`: where the persistent printer configs will be stored
* `/services`: where the Avahi service files will be generated

### Variables:
* `CUPSADMIN`: the CUPS admin user you want created, if not set, the default "cupsadmin" will be used
* `CUPSPASSWORD`: the password for the CUPS admin user, if no set, the name of the admin user will be used
* `TZ`: the local time zone in the _Area/Location_-Format like "Europe/Berlin" to adjust the time stamps in the job list to the local tine, if not set, UTC will be used


### Ports/Network:
* Must be run on host network. This is required to support multicasting which is needed for Airprint.

### Example run command:
```
docker run --name cups-airprint --restart unless-stopped  --net host \
  -v <your services dir>:/services \
  -v <your config dir>:/config \
  -e CUPSADMIN="<username>" \
  -e CUPSPASSWORD="<password>" \
  -e TZ="<timezone>" \
  thoschworks/cups-airprint-bjnp:latest
```

## Add and set up printer:
* CUPS will be configurable at http://[host ip]:631 using the CUPSADMIN/CUPSPASSWORD.
* Make sure you select `Share This Printer` when configuring the printer in CUPS.
* ***After configuring your printer, you need to close the web browser for at least 60 seconds. CUPS will not write the config files until it detects the connection is closed for as long as a minute.***

## Credits

* [chuckcharlie](https://github.com/chuckcharlie/cups-avahi-airprint) for the enhancements on the project
* [quadportnick](https://github.com/quadportnick/docker-cups-airprint) for the original repository
* [tjfontaine](https://github.com/tjfontaine/airprint-generate) for `airprint-generate.py`
* [mainto](https://github.com/mainto/cups-avahi-airprint) for the inspirations how to ["reUbuntunize" the Dockerfile](https://github.com/thosch66/cups-airprint-bjnp/commit/40122e13d171f6fec7b9f3624d042f0a2d0271fc)
