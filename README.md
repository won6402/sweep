# libsweep

C99 library for Scanse Sweep LiDAR.

### Quick Start

    cd libsweep
    make
    sudo make install
    sudo ldconfig

### libsweep

Table of Contents:
- [Version and ABI Management](#version-and-abi-management)
- [Error Handling](#error-handling)
- [Device Interaction](#device-interaction)
- [Full 360 Degree Scan](#full-360-degree-scan)


#### Version And ABI Management

##### `SWEEP_VERSION_MAJOR`

Interface major version number.

##### `SWEEP_VERSION_MINOR`

Interface minor version number.

##### `SWEEP_VERSION`

Combined interface major and minor version number.

##### `int32_t sweep_get_version(void)`

Returns the library's version (see `SWEEP_VERSION`).
Used for ABI compatibility checks.

##### `bool sweep_is_abi_compatible(void)`

Returns true if the library is ABI compatible.
This check is done by comparing the interface header with the installed library's version.


#### Error Handling

##### `SWEEP_ASSERT`

Optionally define this for custom assertion handling.
Uses `assert` from `<assert.h>` by default.

##### `sweep_error_s`

Opaque type representing an error.
You can get a human readable representation using the `sweep_error_message` function.
You have to destruct an error object with `sweep_error_destruct`.

##### `const char* sweep_error_message(sweep_error_s error)`

Human readable representation for an error.

##### `void sweep_error_destruct(sweep_error_s error)`

Destructs an `sweep_error_s` object.


#### Device Interaction

##### `sweep_device_s`

Opaque type representing a Sweep device.
All direct device interaction happens on this type.

##### `sweep_device_s sweep_device_construct_simple(sweep_error_s* error)`

Constructs a `sweep_device_s` using defaults to detect the hardware.
In case of error a `sweep_error_s` will be written into `error`.

##### `sweep_device_s sweep_device_construct(const char* port, int32_t baudrate, int32_t timeout, sweep_error_s* error)`

Constructs a `sweep_device_s` with explicit hardware configuration.
In case of error a `sweep_error_s` will be written into `error`.

##### `void sweep_device_destruct(sweep_device_s device)`

Destructs an `sweep_device_s` object.

##### `void sweep_device_start_scanning(sweep_device_s device, sweep_error_s* error)`

Signals the `sweep_device_s` to start scanning.
In case of error a `sweep_error_s` will be written into `error`.

##### `void sweep_device_stop_scanning(sweep_device_s device, sweep_error_s* error)`

Signals the `sweep_device_s` to stop scanning.
In case of error a `sweep_error_s` will be written into `error`.

##### `int32_t sweep_device_get_motor_speed(sweep_device_s device, sweep_error_s* error)`

Returns the `sweep_device_s`'s motor speed in Hz.
In case of error a `sweep_error_s` will be written into `error`.

##### `void sweep_device_set_motor_speed(sweep_device_s device, int32_t hz, sweep_error_s* error)`

Sets the `sweep_device_s`'s motor speed in Hz.
In case of error a `sweep_error_s` will be written into `error`.

##### `int32_t sweep_device_get_sample_rate(sweep_device_s device, sweep_error_s* error)`

Returns the `sweep_device_s`'s sample rate in S/s.
In case of error a `sweep_error_s` will be written into `error`.

##### `void sweep_device_reset(sweep_device_s device, sweep_error_s* error)`

Resets the `sweep_device_s` hardware.
In case of error a `sweep_error_s` will be written into `error`.


#### Full 360 Degree Scan

##### `sweep_scan_s`

Opaque type representing a single full 360 degree scan from a `sweep_device_s`.

##### `sweep_scan_s sweep_device_get_scan(sweep_device_s device, int32_t timeout, sweep_error_s* error)`

Blocks up to `timeout` milliseconds waiting for the `sweep_device_s` to accumulate a full 360 degree scan into `sweep_scan_s`.
In case of error or timeout violation a `sweep_error_s` will be written into `error`.

##### `void sweep_scan_destruct(sweep_scan_s scan)`

Destructs a `sweep_scan_s` object.

##### `int32_t sweep_scan_get_number_of_samples(sweep_scan_s scan)`

Returns the number of samples in a full 360 degree `sweep_scan_s`.

##### `int32_t sweep_scan_get_angle(sweep_scan_s scan, int32_t sample)`

Returns the angle for the `sample`th sample in the `sweep_scan_s`.

##### `int32_t sweep_scan_get_distance(sweep_scan_s scan, int32_t sample)`

Returns the distance for the `sample`th sample in the `sweep_scan_s`.


### License

Copyright © 2016 Scanse LLC
Copyright © 2016 Daniel J. Hofmann

Distributed under the MIT License (MIT).
