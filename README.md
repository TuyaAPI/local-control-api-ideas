# local-control-api-ideas
Brainstorming a local control interface for Tuya devices

See: https://github.com/codetheweb/tuyapi/discussions/501

Note that everything described below are just ideas / suggestions, even though they may be phrased differently. Take them or leave them.

# Overview

There should be two layers to the API. One for low-level, direct communcation where the API consumer manually sets and decodes / encodes DPS data. Then, there should be a layer built on top of that to facilitate easier use. For example, a lower-level call may look like `set({dps: 2, value: 'ffff8006ff000000'})`; whereas a higher-level call might look like `fadeTo({hex: '#ffffff'})`. This allows the API consumer to choose what level of abstraction they want for their specific use-case.

## The driver (low level)

- The device's state should be continuously and proactively updated & stored in a property rather than being fetched through a function (i.e. `print(device.dps)` instead of `print(device.get())` which would send a request to the device and be blocking).
- As many error cases as possible need to be covered (device disconnects from network, misses the ocasional heartbeat, sends bad data back, etc). When thrown, these errors need to be descriptive and actionable. `Error: device didn't understand request` is far better than `Error: bad data`.

## The library on top (high level)

- There should be convenience functions for every device type that the API consumer can use. For example, `fadeTo()` and `flash()` might be functions for lights. `setHumidityLevel` might be a function for dehumidifiers.
- Each device type should be structured as a plugin, so that the consumer can add their own / write custom plugins if they want.
- Ideally, the device type / schema would be autodiscovered. If that's not possible, it should be easy for the consumer to specify the device type.
