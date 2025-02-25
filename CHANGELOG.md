## 2.0.8

* Small change in the script that starts the application inside the container, to avoid warnings about CHECK_UPDATES in HAOS logs (when running the container as an add-on).

## 2.0.7

* Disabled the option to check for updates when running as a HAOS add-on, since HA already manages the updates of add-ons.
* Minor code refactoring.

## 2.0.6

* Code and repository update to allow the docker image to be installed as an add-on in HAOS.
* Created the <VERSION> tag for the docker image, which contains the multiarch version of the application. Using this tag, it can be installed directly in any of the supported platforms without the need to specify the correct architecture.
* Removed the HEALTHCHECK from the container: it doesn't provide a great benefit and ends up delaying the initialization when running as a HAOS add-on.

## 2.0.5

* Upgraded Rust version to 1.82.0.
* Upgraded some libraries.
* Created a "multiarch" image for docker, containing both AMD64 and ARM64v8.

## 2.0.4

* Upgraded Rust version to 1.81.0.
* Corrected two bugs caused by corner cases, where incorrect values for "Total gas usage" and "Total water usage" could be sent to Home Assistant.
* Added more debug logging entry points.

## 2.0.3

* Changed the retain flag for the new "Total gas usage" and "Total water usage" sensors, so that they are reported correctly when Home Assistant restarts.
  Previously, the value reported by the MQTT server to Home Assistant after a reboot was the value recorded when the Rinnai2mqtt plugin first connected to
  the MQTT server, causing incorrect calculations in the "Energy" dashboard.

## 2.0.2

* Added more debug logging entry points.

## 2.0.1

* Changed the client ID reported to the MQTT server.

## 2.0.0

* Upgraded Rust version to 1.79.0.
* Replaced MQTT client library with Paho MQTT.
* Created two new sensors for Home Assistant. See the docker hub documentation for details.
* Added support for SQLite, in order to store data neeeded for new sensors.
* Some general code refactoring.

## 1.4.2

* Included two new environment variables, DEVICE_MIN_TEMP and DEVICE_MAX_TEMP, to hint Home Assistant about the device's operating temperature range.
  See the docker hub documentation for details.

## 1.4.1

* Small code refactoring in some source files.
* Change the way the application connects to MQTT server on startup. Now, it retries indefinitely until the connection succeeds.
  This behaviour seems to be better for docker apps that depend on an MQTT server running on another container, since the order of
  startup of containers may cause this integration to fail and exit on the first try. In scenarios where Home Assistant and other
  integrations run in docker, this behaviour makes sense when the whole machine holding the containers is rebooted, for example.

## 1.4.0

* Major refactoring of MQTT client code, to optimize resources and to be more efficient.

## 1.3.6

* Included two variables to authenticate with the MQTT server. See the docker hub documentation for details.

## 1.3.5

* Upgraded Rust version to 1.73.0.
* Change the way of sending messages for the "Message" sensor.

## 1.3.4

* Improvements in acquiring one specific mutex for getting results from the device.
* In the updater thread, removes all newline chars when retrieving data from github.

## 1.3.3

* Change in the routine that retrieves last usage statistics from the device, to adjust values that are the same as the previous ones.
  This is because Home Assistant does not report a new value when it is equal to the previous one. So, to overcome this behavior, repeated
  values are subtracted a little bit: by 1 for "last duration", by 1.0 for "last water usage" and by 10.0 for "last gas usage". The amount
  changed is almost negligible, assuming the likelihood of having the exact same values in subsequent utilizations.

## 1.3.2

* Corrected mapping for values reported by the device, which are used to define the mode (heat, off).
* The updater process now reports even when there are no updates available (just to prove that it is alive).

## 1.3.1

* Small code refactoring in the function that processes messages sent to HA.
* Corrected small bug introduced in 1.3.0, regarding the MQTT polling process.

## 1.3.0

* Code refactoring for better handling background tasks.
* Code refactoring in the update check function.
* Small change in how errors collected from device are returned to HA.
* Change the way MQTT discovery messages are sent to HA, to avoid "unavailable" state after HA restarts.
* Change the way MQTT polling is made, to provide better response time in message processing and also in case of program termination.

## 1.2.1

* Added option to disable check for updates (See documentation on docker hub for details).
* Better treatment of errors triggered by the update verification thread.

## 1.2.0

* Added sensor for displaying errors reported by the device (same error code shown in device display).
* Added sensor for showing application version updates. The download and installation process must be made manually, though.

## 1.1.3

* Added HEALTHCHECK to the container.
* Sensors that never have zero values (last gas and water use, for example) now report the last valid value instead of zero
  when the API returns error.
