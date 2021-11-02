# Red Hat OpenShift Streams for Apache Kafka Demo
Red Hat OpenShift Streams for Apache Kafka is a managed cloud service that provides a streamlined developer experience for building, deploying, and scaling real-time applications in hybrid-cloud environments. 

Red Hat OpenShift Streams for Apache Kafka (RHOSAK) makes it easy to create, discover, and connect to real-time data streams no matter where they are deployed. This article helps you to get started with RHOSAK quickly.

## Provision a RHOSAK Kafka instance

### Create a Red Hat account
You need a Red Hat account to provision a managed Kafka instance and to access the Developer Sandbox for Red Hat OpenShift. If you don’t have a Red Hat account, this is how you can create one: Go to console.redhat.com. Click the ‘Register for a Red Hat account’ link to create a Red Hat account. Be sure to select a Personal account type.

![image](https://user-images.githubusercontent.com/8802830/139805458-599d8c02-df69-43d1-8699-d36da7038659.png)

### Provision a Kafka instance
1. Go to console.redhat.com and log in with your Red Hat account. If this is the first time you use your Red Hat account you will have to review and accept the terms and conditions.
2. On the console.redhat.com landing page, select  “Application Services” from the menu.
3. On the Application Services landing page, select “Streams for Apache Kafka - Kafka Instances” or click the “Try OpenShift Streams for Apache Kafka” button in the banner. 
![image](https://user-images.githubusercontent.com/8802830/139805701-c0ea8ded-9226-4f10-b26a-0d711616d62e.png)

4. On the Kafka Instances page, click the “Create Kafka instance” blue button. This will open a wizard, as represented in the picture below, where you’ll be able to choose a name for your Kafka instance, decide on a cloud provider and region options. For the purpose of this demo, please choose all default values. As soon as you click the blue button of “Create Kafka instance” on the wizard, a managed Kafka instance will be provisioned for you in just minutes.

![image](https://user-images.githubusercontent.com/8802830/139806473-62055665-fc45-4526-8219-f458e8e9d638.png)

5. The provisioning process will take a few minutes, and you’ll get a few messages on screen describing the process. Until, you’ll see the green check mark signaling that you now have a Kafka instance. 

## Connect and access the RHOSAK Kafka instance

### Create a service account to connect to RHOSAK Kafka instance
To connect your applications or services to a Kafka instance in the Streams for Apache Kafka web console, you need to create a service account, copy and save the generated credentials, and copy and save the bootstrap server endpoint. You’ll use the service account information later when you configure your application.

1. In the Kafka Instances page of the web console, for the relevant Kafka instance that you want to connect to, select the options icon (three vertical dots) and click View connection information.

2. In the Connection page, copy the Bootstrap server endpoint to a secure location. This is the bootstrap server endpoint that you'll need for connecting to this Kafka instance.

3. Click Create service account to generate the credentials that you'll use to connect to this Kafka instance. Provide a name, for example my-service-account. You can leave the description field empty. Click Create.

4. Copy the generated Client ID and Client Secret to a secure location.

IMPORTANT: The generated credentials are displayed only one time, so ensure that you have successfully and securely saved the copied credentials before closing the credentials window.

5. After you save the generated credentials to a secure location, select the confirmation check box in the credentials window and close the window.

You'll use the boostrap server and service account information that you saved to configure your application to connect to your Kafka instances when you're ready. For example, if you plan to use Kafkacat to interact with your Kafka instance, you'll use this information to set your bootstrap server and client environment variables.


### Setting permissions for a service account in the RHOSAK Kafka instance
1. Click the Access tab to view the current ACL for this instance.

2. Click Manage access, use the Account drop-down menu to select the All accounts. Click Next.

3. Under Assign Permissions, use the drop-down menus to set the permissions shown in the following table for all accounts. Click Add to add each new resource permission.

These permissions enable all applications with a valid service account to create and delete topics in the instance, to produce and consume messages in any topic in the instance, and to use any consumer group and any producer.

Example ACL permissions:

Topic: Is = *, Allow, All
Consumer group: Is = *, Allow, Read
Transactional ID: Is = *, Allow, All
After you add these permissions for the service account, click Save to finish.

### Create a Kafka topic in RHOSAK
When you have a Kafka instance available, you can create Kafka topics to start producing and consuming messages in your services.

1. Click Create topic and follow the guided steps to define the topic details. Click Next to complete each step and click Finish to complete the setup.

Topic name: Enter a unique topic name, such as my-first-kafka-topic.

Partitions: Set the number of partitions for this topic. This example sets the partition to 1 for a single partition. Partitions are distinct lists of messages within a topic and enable parts of a topic to be distributed over multiple brokers in the cluster. A topic can contain one or more partitions, enabling producer and consumer loads to be scaled.

NOTE: You can increase the number of partitions later, but you cannot decrease them.

Message retention: Set the message retention time to the relevant value and increment. This example sets the retention to 1 day. Keep the Retention size set to Unlimited (the default). Message retention time is the amount of time that messages are retained in a topic before they are deleted or compacted, depending on the cleanup policy.

Replicas: Set the number of partition replicas for the topic and the minimum number of follower replicas that must be in sync with a partition leader. In the current version of Streams for Apache Kafka the values are fixed and set to 3 for the number of replicas and 2 for the in-sync replicas. Replicas are copies of partitions in a topic. Partition replicas are distributed over multiple brokers in the cluster to ensure topic availability if a broker fails. When a follower replica is in sync with a partition leader, the follower replica can become the new partition leader if needed.

After you complete the topic setup, the new Kafka topic is listed in the topics table. You can now start producing and consuming messages to and from this topic using services that you connect to this instance.

### Download Kafka CLI
Download Kafka (including CLI) from https://kafka.apache.org/downloads and extract it

### Create a file kafka.properties with the connection details such as below
```
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="<Client ID>" password="<Client Secret>";
```

### Produce messages using Kafka CLI
```
bin/kafka-console-producer.sh --bootstrap-server rlui-kafka-c-vovcoabgjhdqealcsa.bf2.kafka.rhcloud.com:443  --topic my-first-kafka-topic --producer.config kafka.properties
>Message1
>Message2
>Message3
```
Press Ctrl-C to exit

### Consume message using Kafka CLI
```
.bin/kafka-console-consumer.sh --bootstrap-server rlui-kafka-c-vovcoabgjhdqealcsa.bf2.kafka.rhcloud.com:443  --topic my-first-kafka-topic --consumer.config kafka.properties --partition 0 --offset 'earliest'
```
Press Ctrl-C to exit






