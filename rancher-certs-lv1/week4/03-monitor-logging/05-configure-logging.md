# Configure Logging
Logging is available at both the cluster and project levels and can be enabled at either level, independent of the other.

Rancher collects stdout and stderr output from each container, along with any logs written to /var/log/containers on the hosts and sends it to the configured endpoint.

When enabled at the project level, only logs for that project's workloads are collected.

When enabled at the cluster level, all logs for every workload are collected.

Rancher can write logs to Elasticsearch, Splunk, Fluentd, Kafka, or syslog.
