# Heathkit ID-5001 for WeeWX

Read the Heathkit ID-5001 Weather Computer through its serial interface option.
The driver polls indoor/outdoor temperature and humidity, wind, gust, pressure
and rain. Serial communication uses 9600 baud, 8 data bits, no parity, one stop
bit and no flow control.

Copyright 2017 Mark A. Matthews. Derived from Tom Keffer's simulator and the
original author's wview driver. Distributed under [GPLv3](LICENSE).

This repository mirrors upstream source. For WeeWX 5, install the separately
repaired catalog archive **0.1.post1** linked below. A branch download
does not include those catalog repairs.

## Install

Use the Python environment and `weectl` belonging to your WeeWX station. The catalog release requires Python >=3.10,<3.15 and WeeWX >=5.5,<6.

Install `pyserial` in the WeeWX Python environment:

```sh
python -m pip install pyserial
```

```sh
weectl extension install https://github.com/weewx-packages/catalog/releases/download/id5001/v0.1.post1/id5001-0.1.post1.tar.gz --config=/path/to/weewx.conf
weectl station reconfigure --driver=user.id5001 --config=/path/to/weewx.conf
```

Replace `/path/to/weewx.conf` with your station configuration. A downloaded copy
of this same archive can be passed instead of the URL. These commands work with
stock WeeWX 5; no package-manager integration is required.

## Configure serial acquisition

```ini
[Station]
    station_type = ID5001
[ID5001]
    driver = user.id5001
    port = /dev/ttyUSB0
    model = ID5001
    loop_interval = 5.0
```

Select your actual serial port, grant the WeeWX service user access and restart
WeeWX. The repaired archive includes an installer and puts the module in
`bin/user/id5001.py`. Its configuration editor uses `user.id5001`; the mirrored
standalone source still contains the older `weewx.drivers.id5001` default.

Opening the driver initializes the station and clears its peak gust for the
next polling cycle. Rain increments come from the station's running counter.
The code emits `rain_rate` and misspelled `windhcill`, rather than standard
`rainRate` and `windchill`; do not expect those derived values in standard reports.
No hardware test is claimed. The module's `--help` exits before opening a port:

```sh
PYTHONPATH=/path/to/station/bin python -m user.id5001 --help
```

Original source: [n2ee/weewx-id5001](https://github.com/n2ee/weewx-id5001).

<!-- Documentation updated 2026-10-23 by weewx-packages. -->
