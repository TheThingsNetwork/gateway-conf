# The Things Network Frequency Plans

This repository is an attempt at formatting the Things Network's frequency plans in a YAML format. Those frequency plans are derived from the LoRaWAN regional parameters.

Until now, frequency plans were stored in `global_conf.json` files, that were storing a lot of information relevant for different setups: concentrator-specific information, gateway-specific information, network server-specific information... This repository is an attempt at achieving clear separation of concerns.

+ [Format](#format)
+ [Index format](#index-format)
    + [Frequency plans extensions](#frequency-plans-extensions)

## Format

```yml
band-id: "BAND_ID" # ID of the band
channels: # List of channels
- frequency: 863000000 # Channel frequency
  data-rate: # Optional: If forcing a data rate for this specific frequency
    index: 3 # Index of the data rate to force within the region's data rates list
lora-std-channel: # Optional: specify a LoRa standard channel, according to the same specs than the previous frequency plans. Follows the same format than other channels.
  frequency: 863000000
  data-rate: # Which data rate this channel should listen on
    index: 6
FSK-channel: # Optional: specify a FSK channel, according to the same specs than the previous frequency plans. Follows the same format than other channels.
  frequency: 868800000
  data-rate:
    index: 7
lbt: # Optional: Enabling listen-before-talk
  rssi-offset: 0 # Optional: RSSI offset
  rssi-target: -80 # Optional: RSSI offset
  scan-time: 128 # Scan time in micro-seconds
tx-timeoff-air: # Optional: Cooloff time after TX emission, either in fraction, either in absolute duration
  fraction: 0.1 # Fraction of the time on air to account as cooloff after emission (here: 10% of time on air)
  duration: 1s # Absolute cooloff time, in Go-parsable format https://golang.org/pkg/time/#ParseDuration (here: 1 second)
dwell-time: 800ms # Optional: maximum possible duration of a transmission
```

## Index format

`frequency-plans.yml` is used by The Things Network services to determine which frequency plans to load, from which files, and how to display them.

```yml
- id: AS_923_925 # Unique ID of the frequency plan
  description: Asia 923-925Mhz frequency plan # Description
  base_freq: 923 # Base frequency in MHz
  file: AS2_923_925.yml # Filename
- id: JP
  description: Japanese 923MHz frequency plan
  base_freq: 923
  file: JP.yml
  base: AS_923_925 # Base frequency plan ID for extensions
```

#### Frequency plans extensions

In the YAML frequency plan format, you can define frequency plans as **extensions** of others. Those extensions are defined in the index (`frequency-plans.yml`) file. The frequency plan file is then used to know what extensions are to be applied. For example, to have a JP frequency plan that extends the AS2 frequency plan by adding Listen-before-talk:

```yml
lbt: # Optional: Enabling listen-before-talk
  rssi-offset: 0 # Optional: RSSI offset
  rssi-target: -80 # Optional: RSSI offset
  scan-time: 128 # Scan time in micro-seconds
```

*JP.yml*

Extensions are determined by reading what top-level parameters are set, and by overriding them if they're set.
