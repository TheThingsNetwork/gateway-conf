# TTN Master Gateway Configurations

This repository contains global gateway configuration files for LoRaWAN gateways to be connected to The Things Network.

It is intended that these will form the basis for the "global_conf.json" configuration files, and that the parameters contained herein are *only* those that pertain to regulatory policy, TTN policy, or TTN operations.

All parameters related to device operations should appear within the gateway's "local_conf.json".  Although technically possible, TTN strongly discourages gateways from overriding the parameters contained in these TTN global configuration files.

Due to the difficulty of updating gateways in the field, we recommend that this configuration be dynamically loaded by every gateway upon restart.  This will enable TTN to, for example, update servers, ports, or frequency plans as necessary.

Each gateway should be configured with a two-letter "region code" from which the configuration file name can be derived and subsequently loaded using HTTPS. For example, if the configured region is "EU", the gateway will then load and initialize the gateway/s "global_conf.json" file (i.e. using CURL) from:

https://raw.githubusercontent.com/TheThingsNetwork/gateway-conf/master/EU-global_conf.json

## Structural Configurations and Geographic Configurations

Although the AS2 region plan uses a consistent set of frequencies, some countries require that the gateway avoid transmitting on a given channel if it detects a signal on that channel. This capability is called "listen-before-talk" (or "LBT").  Japan requires listen-before-talk to be implemented, while other countries (for example, Taiwan) do not. Many gateways do not provide listen-before-talk in hardware, and the gateway will fail to operate if listen-before-talk is requested by the configuration file.

To make it easy to configure gateways, we use an approach similar to that used by POSIX systems for time zones. We define "Structural Configurations" that combine all the required rules, and geographic configurations as synonyms for the structural configuration appropriate to the country in question.

For AS1 and AS2, we define the following structural configurations.

1. `AS1-lbt125-global_conf.json` configures the gateway for the AS1 region, with 125 microsecond, -80 dB listen before talk.

2. `AS2-lbt125-global_conf.json` configures the gateway for the AS2 region, with 125 microsecond, -80 dB listen before talk.

3. `AS1-nolbt-global_conf.json` configures the gateway for the AS1 region, without LBT.

4. `AS2-nolbt-global_conf.json` configures the gateway for the AS2 region, without LBT.

(`AS1-global_conf.json` and `AS2-global_conf.json` are equivalent to the LBT configurations, and are provided for backwards compatibility.)

For specific countries, where we know the policies, we define the country using a two letter "region" code.

 Code | Country | Full file name        | Equivalent Structural Configuration
:----:|:-------:|:----------------------|--------------------
  JP  | Japan   | `JP-global_conf.json` | `AS2-lbt125-global_conf.json`
  TW  | Taiwan  | `TW-global_conf.json` | `AS2-nolbt-global_conf.json`

## Use with MultiTech Conduits

NOTE: These config files cannot be used with the poly_pkt_fwd for MultiTech Conduits without modifications! The forwarder will not start/run when attempting to use the files as they are.
