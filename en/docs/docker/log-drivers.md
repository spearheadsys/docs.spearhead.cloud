# Log Drivers

> Last edit: Fri Oct 1 16:13:20 EEST 2021

With spearhead-docker there is limited support for the --log-driver and --log-opts functions. Important differences include:

* spearhead-docker does not support the 'journald' driver at all
* the '--log-opt syslog-address' can only be used with the tcp/udp format. The unix://path format is unsupported as that expects to write to arbitrary host locations.
* the 'syslog-address' option is required when using the syslog log-driver
* the 'fluentd-address' option is required when using the fluentd log-driver
* when using any log drivers other than 'json-file' and 'none', additional processes will be running in your container to handle the logging. All hosts specified will be resolved from the container. This allows logging for example on a vxlan network which may not be exposed elsewhere.
