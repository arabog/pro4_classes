Exercise: Prometheus Monitoring
Monitoring is an essential component of DevOps best practices. In this exercise, you will set up Prometheus Monitoring.

Alerting Theory
For this exercise you will:

Set up Prometheus monitoring.
Consider a normal distribution, "six sigma" and the 68-95-99.7 rule. Computer systems events are often normally distributed, meaning that all events within three standard deviations from the mean occur with 99.7 of the occurrences.
https://en.wikipedia.org/wiki/Six_Sigma
https://en.wikipedia.org/wiki/68%E2%80%9395%E2%80%9399.7_rule
https://en.wikipedia.org/wiki/Normal_distribution

Design a process that alerts senior engineers when events are greater than three standard deviations from the mean and write up how the alerts should work, i.e.
Who should get a page when an event is more significant than three standard deviants from the mean?
Should there be a backup person who gets alerted if the first person doesn’t respond within five minutes?
Should an alert wake up a team member at one standard deviation? What about two?

Prometheus is a popular metrics and alerting solution that is often used with containers and Kubernetes. To run this tutorial please do the following.

Use a local environment or preferably AWS Cloud9. If you use AWS Cloud9, you will need to expose port 9090 via EC2 Security Group.
Download, install and run Prometheus. On AWS Cloud9 you would install the latest release that has *.linux-amd64.tar.gz in the name. This would like something like wget <some release>.linux-amd64.tar.gz.
tar xvfz prometheus-*.tar.gz
cd prometheus-*
Configure Prometheus by creating a prometheus.yml file

global:
  scrape_interval:     15s
  evaluation_interval: 15s

rule_files:
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ['localhost:9090']
Start Prometheus
Wait about 30 seconds for Prometheus to collect data.

./prometheus --config.file=prometheus.yml
View data through the expression browser
Go to http://localhost:9090/graph. Choose the "console" within the graph. One metric that Prometheus collects is how many times http://localhost:9090/metrics has been called. If you refresh a few times then type the following in the expression console you can see a time series result.

promhttp_metric_handler_requests_total
View data through the graphing interface
Another way to view data is via the graphing interface. Go to http://localhost:9090/graph. Use the "Graph" tab.

rate(promhttp_metric_handler_requests_total{code="200"}[1m])
(OPTIONAL) Going further, feel free to experiment with how that would work by following the example below and changing to suite your needs.
A more sophisticated example would involve also collecting data from clients. Next download these go clients using the code below and run them.

# Fetch the client library code and compile example.
git clone https://github.com/prometheus/client_golang.git
cd client_golang/examples/random
go get -d
go build

# Start 3 example targets in separate terminals:
./random -listen-address=:8080
./random -listen-address=:8081
./random -listen-address=:8082
Next, add these clients in the prometheus.yml

scrape_configs:
  - job_name:       'example-random'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:8080', 'localhost:8081']
        labels:
          group: 'production'

      - targets: ['localhost:8082']
        labels:
          group: 'canary'
Restart prometheus and view this metric in the expression browser.

rpc_durations_seconds

Based on guide from official Prometheus documentation and guide here
https://github.com/prometheus/docs/blob/432af848c749a7efa1d731f22f73c27ff927f779/content/docs/introduction/first_steps.md

