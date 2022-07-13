-: Alerts and Incidents Response
Operationalizing a Microservice Overview
One important factor in developing a microservice is to think about the feedback loop. In this diagram, a GitOps style workflow is described.
https://queue.acm.org/detail.cfm?id=3237207

Application is stored in Git.
Changes in Git trigger the continuous delivery server which then tests and deploys the code to a new environment. This environment is configured as Infrastructure as Code (IaC).
The microservice, which could be a containerized service running in Kubernetes or a FaaS (Function as a Service) running on AWS Lambda, has logging, metrics, and instrumentation.
A load test using a tool like locust.
https://locust.io/

When the performance and auto-scaling is verified the code is merged to production and deployed

What are some of the items that could be alerted on with Kubernetes?
Alerting on application layer metrics
Alerting on services running on Kubernetes
Alerting on the Kubernetes infrastructure
Alerting on the host/node layer

How could you collect metrics with Kubernetes and Prometheus? Here is a diagram that walks through a potential workflow. Note that there are two pods. One pod is dedicated to the Prometheus collector and the second pod has a "sidecar" Prometheus container that sits alongside the Flask application. This all propagates up to a centralized monitoring system that visualizes the health of the clusters and trigger alerts.

Another helpful resource is an official sample project from Google Cloud Monitoring apps running on multiple GKE clusters using Prometheus and Stackdriver.
https://cloud.google.com/architecture/monitoring-apps-running-on-multiple-gke-clusters-using-prometheus-and-stackdriver

Reference
Monitor Node Health
https://kubernetes.io/docs/tasks/debug/debug-cluster/monitor-node-health/

Creating Effective Alerts
At one company I worked at there was a homegrown monitoring system (again, initially created by the founders) that alerted on average every 3-4 hours, 24 hours a day.

Because everyone in engineering, except the CTO, was on call, most of the engineering staff was always sleep deprived. This system guaranteed that every night there were alerts about the system not working. The "fix" to the alerts was to restart services. I volunteered to be on call for one month straight to allow engineering the time to fix the problem. This sustained period of suffering and lack of sleep led me to realize several things: one, the monitoring system was no better than random; two, I could potentially replace the entire system with a random coin flip.

Even more distressing, when looking at the data, it was clear that engineers had spent YEARS of their lives responding to pages and getting woken up at night. All that, and it was utterly useless. The suffering and sacrifice accomplished nothing and reinforced the sad truth that life is not fair. The unfairness of the situation was quite depressing, and it took quite a bit of convincing to get people to agree to turn off the alerts. There is a built-in bias in human behavior to continue to do what you have always done. Additionally, because the suffering was so severe and sustained, there was a tendency to attribute a deeper meaning to it. Ultimately, it was a false God.

Q: What can be worse than not having alerts for critical failures?
While having alerts is important, they should be targeted, actionable, automated and safe.

-: Disaster Recovery
An important but often overlooked part of building production software is designing for failure. There is an expression that the only two certainties in life are death and taxes. We can add another certainty to the list, software system failure. In the AWS whitepaper Serverless Application Lens they discuss five pillars of a well architected serverless system: operational excellence, reliability, performance efficiency and cost optimization. It is highly recommended to read this guide thoroughly.

https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html

Five Pillars of a Well Architected Serverless System
Operational Excellence
How do you understand the health of a serverless application? One method is to use metrics and alerts. These metrics could include business metrics, customer experience metrics and other metrics. Another method is to have centralized logging. This allows for unique transactions ideas that can narrow down critical failures.

Security
Have proper controls and using the POLP (Principle of Least Privilege). Only give out the privileges needed to complete a task to a user, service or developer. Protect data at rest and in transit.

Reliability
Plan on the fact that failure will occur. Have retry logic for key service and build a system that can queue work when a service is unavailable. Use highly available services that store multiple copies of the data like Amazon S3 and then archive key data to services that can store immutable backups. Ensure that you have tested these backups and validated them on a recurring basis (say quarterly)

Performance
One key way to validate performance is to load test an application that has proper instrumentation and logging. Several load test tools include: Apache Bench, Locust and load test services like loader.io
https://httpd.apache.org/docs/2.4/programs/ab.html
https://locust.io/
https://loader.io/

Cost
Serverless technologies like AWS Lambda fit well with cost optimization because they are driven by usage. Events trigger the execution of services and this saves a tremendous amount on cost if the architecture is designed to take advantage of this.

Summary of Serverless Best Practices
One of the advantages of serverless application development is that it encourages the use of IaC and GitOps on top of highly durable infrastructure. This means that entire environments can be spun up to mitigate severe unplanned outages in geographic regions. Additionally, the use of automated load testing alongside comprehensive instrumentation and logging leads to environments that are robust in the face of disasters.

Reference
Backup and Migrate Kubernetes: LINK
https://github.com/vmware-tanzu/velero

Q: What are some core principles of Disaster Recovery?
Having more than one copy of critical data, and test-restoring backups regularly, are both key to disaster recovery.

-: CI/CD Pipeline Integration
Earlier in the course, we briefly touched on CircleCI through its inclusion within a Makefile Noah used.

CircleCI is a great tool to help with continuous integration, and it works well with Kubernetes. You can read some information in this CircleCI blog post on one application combining Kubernetes with CircleCI, and on the next page, it will be your turn to add CircleCI to your build.
https://circleci.com/blog/k8s-deployments-with-cloudflare/

Q: What problem does CI/CD solve?
CI/CD helps ensure software is always ready for delivery.


-: Load Testing
On the next page, you'll get a chance to first load test a sample app with Locust, and then work to scale up this sample app to better handle higher loads.

Reference
Locust Helm Chart: LINK
https://github.com/helm/charts/tree/master/stable/locust

-: Lesson Summary
Key Terms:
Alerts
Alerts are health metrics that have actions associated with them. An example would be an alert that sends a text message to a software engineer when a web service returns multiple error status codes.

Operationalization
The process of making an application ready for production deployment. These actions could include monitoring, load-testing, setting up alerts, and load-testing.

Metrics
Metrics are the creation of KPIs (Key Performance Indicators) for a software application. An example of a parameter is the percentage of CPU used by a server.

Disaster recovery
Disaster recovery is the process of designing a software system to recover despite a disaster. This process could include archiving data to another location.

Migrate
Migrate is the ability to move an application from one environment to another.

Continuous Integration
Continuous Integration is the process of automatically testing software upon check in to the source control system.

Continuous Delivery
Continuous Delivery is the process of delivering tested software automatically to any environment.

Load Testing
Load testing is the process of verifying the scale characteristics of a software system.

Locust
Locust is a load-testing framework that accepts Python formatted load test scenarios.

Source: https://noahgift.github.io/cloud-data-analysis-at-scale/topics/key-terms

