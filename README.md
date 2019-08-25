Role Name
=========

The Role create CloutWatch alarms for different AWS services. At the moment, the following services are supported:

* EC2
* RDS
* ELB

Requirements
------------

AWS account

Role Variables
--------------

* **sns_topic**. Name of the SNS topic where the alarms would be sent.
* **minDelayTarget**. Minimum delay allowed before sending to the different targets.
* **maxDelayTarget**. Maximum delay allowed before a timeout is triggered.
* **numRetries**. In case a sending operation fail, this establish the maximum number of retries.
* **numMaxDelayRetries**. In case the send operation fails because of a previous timeout, this establish the maximum number of retries.
* **backoffFunction**
* **disableSubscriptionOverrides**
* **maxReceivesPerSecond**. Throttling parameter to control the flow of requests.
* **subscriptions**. Subscriptions associated with the SNS topic.

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
- name: Create CPU usage alarms for EC2 instances
  include_role:
    name: ansible-cloudwatch-alarm
  vars:
    aws_service: EC2
    aws_region: "{{ aws_mobility_region }}"
    sns_topic: "{{ project_name }}_{{ environment_env }}_{{ aws_service }}_alarms"
    minDelayTarget: 2
    maxDelayTarget: 4
    numRetries: 5
    numMaxDelayRetries: 5
    backoffFunction: "linear"
    disableSubscriptionOverrides: True
    maxReceivesPerSecond: 10
    subscriptions:
      - endpoint: "{{ sns_email_1 }}"
        protocol: "email"
    ec2_group: "{{ project_name }}_{{ environment_env }}_docker"
    metric: "CPUUtilization"
    namespace: "AWS/EC2"
    statistic: Average
    comparison: ">="
    threshold: 70.0
    period: 60
    evaluation_periods: 3
    unit: "Percent"
    description: "This will alarm when the instance have >=70% of CPU usage"
```


Author Information
------------------

[Agustín García Flores](https://www.linkedin.com/in/agust%C3%ADn-garc%C3%ADa-flores-bb9aa975/)
