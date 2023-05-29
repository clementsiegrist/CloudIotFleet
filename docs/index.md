# Archictecture Proposal for IoT Fleet Deployment, Storage and Analysis on AWS

![Architecture Diagram](img/iots_jems_private.drawio(7).png)

-----------------------------------------
                 
## **I. Vehicle Gateway  parameterization**

_IoT Fleewise SDK as well as AWS IoT SDK and other softwares are installed on the computer board of each vehicle and parameterized according to the sensors/or and the computer board specs. Devices are given an individual ID, their model is tagged and they are attributed to a fleet if we want to group them and analyze them later on_.

—-----

[Demo auto aws Iot FleetWise](https://github.com/aws4embeddedlinux/demo-auto-aws-iotfleetwise)<br> 

[Iot-device-simulator](https://github.com/aws-solutions/iot-device-simulator)<br> 

[Iot Device Simulator](https://docs.aws.amazon.com/solutions/latest/iot-device-simulator/source-code.html)<br> 

[IoT Device Simulator Java](https://github.com/aws-solutions/iot-device-simulator)

[Iot Fleetwise Edge Development Code](https://github.com/aws/aws-iot-fleetwise-edge/blob/main/docs/dev-guide/edge-agent-dev-guide.md#aws-iot-fleetwise-quick-start-demo?sc_channel=EL&sc_campaign=Live_Streaming_2022_vid&sc_medium=YouTube&sc_content=Pp_ZkcTlFX4&sc_detail=INTERNET_OF_THINGS&sc_country=US)<br> 

[AWS IoT Fleetwise Insights](https://aws.amazon.com/blogs/iot/generating-insights-from-vehicle-data-with-aws-iot-fleetwise-part1/)<br> 

[AWS IoT Automotive Workshop](https://catalog.workshops.aws/awsiotforautomotive/en-US/0-before-starting-the-workshop)<br>

[AWS Next Gen Vehicle Communication](https://docs.aws.amazon.com/whitepapers/latest/designing-next-generation-vehicle-communication-aws-iot/challenges-with-connected-vehicle-platforms.html)

—-----


### **IoT Fleetwise vs IoT Core as primary data ingestion AWS service**  

AWS IoT FleetWise (formerly AWS IoT FleetHub) and AWS IoT Core are two separate services designed for different purposes within the AWS IoT ecosystem. Here's a comparison of the two services with a focus on their similarities, differences, advantages, drawbacks, and complementarity in the context of managing a worldwide connected vehicle fleet sending terabytes of data per hour:


**1. AWS IoT FleetWise:**

* Purpose: 

    FleetWise is a web application that enables you to monitor, track, and manage your connected vehicle fleet. It provides a centralized platform for fleet management, including real-time vehicle tracking, remote diagnostics, and over-the-air (OTA) updates.

* Advantages: 

    FleetWise simplifies fleet management by providing an easy-to-use interface and built-in features specifically tailored for connected vehicle fleets. It offers real-time monitoring, remote diagnostics, and OTA updates, which can help improve operational efficiency, reduce downtime, and save costs.

* Drawbacks: 

    FleetWise focuses primarily on fleet management tasks and doesn't provide the same level of flexibility, scalability, and integration with other AWS services as IoT Core.

**2. AWS IoT Core:**

* Purpose: 

    IoT Core is a managed cloud service that enables secure and reliable communication between IoT devices and cloud applications. It supports billions of devices and trillions of messages while ensuring low latency and high throughput. IoT Core provides device connectivity, authentication, authorization, device management, and integration with other AWS services.

* Advantages: 

    IoT Core offers robust device connectivity, security, and integration with other AWS services. It is highly scalable and supports a wide range of protocols, making it suitable for managing large connected vehicle fleets sending terabytes of data per hour. IoT Core's rules engine and integration with services like Kinesis, Lambda, and S3 enable efficient data processing, analytics, and storage.

* Drawbacks: 

    IoT Core doesn't provide a dedicated interface for fleet management tasks like FleetWise. While it is possible to build custom fleet management applications using IoT Core, it requires more development effort compared to using the out-of-the-box features provided by FleetWise.

**Complementarity** : 

IoT FleetWise and IoT Core can be complementary in managing a connected vehicle fleet. IoT Core can be used to handle device connectivity, data ingestion, and processing, while FleetWise can serve as the fleet management interface for monitoring, tracking, and managing your connected vehicles. By integrating both services, you can leverage the strengths of each service to create a comprehensive solution for your connected vehicle fleet.

**Choosing the best one**

If your primary requirement is an easy-to-use fleet management solution with built-in features tailored for connected vehicle fleets, AWS IoT FleetWise might be the better choice.

However, if you need a highly scalable and flexible IoT platform that can handle billions of devices, trillions of messages, and integrate with other AWS services for data processing and analytics, AWS IoT Core is the more suitable option.


### **AWS IoT FleetWise Edge Agent** 

Edge Agent software can be installed on sensors embedded in vehicles to collect, process, and analyze sensor data in real-time. It is designed to work in tandem with AWS IoT FleetWise and provide edge computing capabilities for vehicle IoT sensor data processing before transmission to the cloud.

To transfer the data over to the cloud, Edge Agent uses AWS IoT Core. It provides secure, scalable, and cost-efficient MQTT/TLS connectivity. Route the vehicle data to the purpose-built database or storage services. At the time of publishing of this blog, AWS IoT FleetWise supports Amazon Timestream service. Amazon S3 will be supported after GA.

The benefits of using AWS IoT FleetWise Edge Agent are as follows:


* Real-time processing: 

    Edge Agent allows for real-time processing of IoT sensor data, allowing businesses to quickly detect issues and make decisions more rapidly.

* Low latency: 

    Edge processing reduces latency by reducing the time required to transmit data to the cloud and receive a response.

* Reduced data transmission costs: 

    By processing data at the edge, it is possible to reduce the amount of data that needs to be transmitted to the cloud, thereby reducing data transmission costs.

* Security: 

    Edge Agent can be used to implement additional security measures, such as encrypting data in transit.

Compared to data processing on the onboard computer of vehicles using a microcontroller optimized for connected vehicles, using AWS IoT FleetWise and Edge Agent offers several advantages, such as:


* Scalability: 

    AWS IoT FleetWise is designed to handle large-scale vehicle fleets and can easily integrate with other AWS services for data storage, processing, and analysis.

* Flexibility: 

    Businesses can easily add new types of sensors and vehicle data to their IoT architecture without having to modify the entire processing infrastructure.

* Advanced analysis: 

    Businesses can use advanced analysis tools such as Amazon SageMaker to perform predictive analysis on IoT sensor data and make more informed decisions.

In summary, using AWS IoT FleetWise and Edge Agent enables real-time processing of vehicle IoT sensor data, reducing latency and data transmission costs while providing advanced analysis and security features. These benefits, combined with the scalability and flexibility of AWS IoT, make it an attractive solution for businesses managing large fleets of connected vehicles.

Fleetwise is an AWS service that securely manages IoT device certificates and identities. By using Fleetwise, you can create, manage, and revoke certificates for IoT devices, greatly simplifying management of large-scale IoT devices.

To use Fleetwise with a Raspberry Pi, follow these steps:


1. Create an instance of the AWS Private Certificate Authority (PCA) service using AWS Certificate Manager (ACM). This step creates a private certificate authority that can be used to sign IoT device certificates.

2. Create a Thing Group in AWS IoT Core for each vehicle fleet. Thing Groups are collections of IoT devices that share common characteristics.

3. Create device certificates for each Raspberry Pi using Fleetwise. Certificates can be created manually or automatically using a certificate request process.

4. Associate certificates with the corresponding Things in AWS IoT Core.

5. Configure the Raspberry Pis to use certificates to authenticate with AWS IoT Core. This can be done using the AWS IoT SDK for Python.

Using Fleetwise, you can also manage access permissions for IoT devices. You can create access policies and associate them with Thing Groups to control access to AWS resources, such as Kinesis streams or DynamoDB databases.

### OBD Decoders and OBD Nodes

OBD stands for On-Board Diagnostics, a vehicle's self-diagnostic and reporting capability. OBD systems give the vehicle owner or a repair technician access to the status of various vehicle subsystems.

The amount of diagnostic information available via OBD has varied widely since its introduction in the early 1980s. Early versions of OBD would simply illuminate a malfunction indicator light, or "idiot light", if a problem was detected but would not provide any information as to the nature of the problem. Modern OBD implementations use a standardized digital communications port to provide real-time data in addition to a standardized series of diagnostic trouble codes, or DTCs, which allow a person to rapidly identify and remedy malfunctions within the vehicle.

An OBD Decoder is a tool or software that can interpret these diagnostic trouble codes (DTCs) and provide a description for each code. Each code corresponds to a specific error or status update in the vehicle's systems. Decoders are very useful for mechanics and anyone else looking to diagnose and fix mechanical issues, as they provide detailed information about what is going on in the vehicle.

In the context of CAN (Controller Area Network) and OBD, a node can refer to any physical device in a network, including sensors, controllers, or even the OBD port itself. For example, the Engine Control Unit (ECU) can be considered an OBD node, as it communicates with other components in the vehicle through the CAN bus.

### CAN and DBC 

 This is a file format used to describe a Controller Area Network (CAN) system. This format is used by various automotive, industrial, and scientific applications. In a DBC file, the CAN messages and their corresponding signal values are defined.

### **Problems solved with Fleetwise**


1. Implementation complexity due to proprietary data formats. The variety of data formats leads to high complexity of systems needed to analyze vehicle-wide and fleet-wide vehicle data. This complexity results in a high implementation and maintenance effort, often preventing or slowing down implementation of data-driven use cases.


2. AWS IoT FleetWise helps to reduce data volume by providing intelligent data filtering capabilities. With AWS IoT FleetWise, you can reduce data volume in two ways.


3. First, you can configure the vehicle to collect only the signals, that are required for the purpose of your use cases. Second, you can configure AWS IoT FleetWise to collect the signals only under certain conditions. Examples for such conditions are scheduled collection (e.g., only between 1PM and 2PM on a specific date) or condition-based collection (e.g., only when battery temperature is above the threshold).


4. The AWS IoT FleetWise Edge Agent software running in vehicles uses campaigns to decide how to collect and transfer data to the cloud. You create campaigns in the cloud. After you or your team approve campaigns, AWS IoT FleetWise automatically deploys them to vehicles.


## **II. Security, Communication and Governance**


_A  PKI (Public Key Infrastructure) must be established to ensure the authenticity of both the clients (devices) and servers (AWS services and enterprise servers) and the encryption of the communications. Both attestation ( verify authenticity) and operational (establish secure communication) must be issued by a Root Authority, managed by AWS Private CA to ensure communication between IoT devices and IoT Core._


—---------------------------------


### **Certificate Signing Request :** 


CSR stands for Certificate Signing Request, which is a message sent by a client, such as a Vehicle Gateway, to a Certificate Authority (CA) or Certificate Broker requesting a digital certificate. In the context of IoT security, a digital certificate is used to authenticate devices and ensure secure communication between them.


When a Vehicle Gateway sends a CSR to a Certificate Broker, it includes information about the device, such as its unique identifier, public key, and other metadata. The Certificate Broker then uses this information to issue a digital certificate that is specific to the Vehicle Gateway. This digital certificate is signed by the Certificate Authority, indicating that the certificate can be trusted.


Once the Vehicle Gateway receives the digital certificate from the Certificate Broker, it can use this certificate to establish a secure connection with other devices or services, such as an IoT cloud platform or other vehicles in a fleet. The certificate serves as proof of identity and authenticity, ensuring that only authorized devices can access the system.


In summary, the CSR communication between a Vehicle Gateway and a Certificate Broker is an important step in the process of establishing secure communication between IoT devices. It enables the Vehicle Gateway to obtain a digital certificate that can be used to authenticate the device and establish secure connections with other devices and services.

### **Attestation and operational certificate**

Attestation and operational certificates are two different types of certificates used in the context of IoT security.


**Attestation certificates** are used to verify the identity and authenticity of a device. They are typically used during the device onboarding process, where a device sends its identity information to a certificate authority or certificate broker. The certificate authority or broker then issues an attestation certificate that verifies the device's identity and ensures that it is trusted by the system. Attestation certificates are typically used in the initial stages of device provisioning and are only valid for a limited time.


**Operational certificates**, on the other hand, are used to **establish secure communication channels between devices and services once the device is operational. They are used to encrypt data sent between devices and ensure that it can only be read by authorized parties. Operational certificates are typically issued by a certificate authority or broker and can be valid for a longer period of time than attestation certificates.

In summary, attestation certificates are used to verify the identity and authenticity of a device during the onboarding process, while operational certificates are used to establish secure communication channels between devices and services once the device is operational. Both types of certificates play a critical role in securing IoT devices and ensuring that they can communicate securely and reliably with other devices and services in the IoT ecosystem.


### **AWS Private CA**

AWS Private CA (Certificate Authority) is a fully-managed private certificate authority service that enables you to create and manage private certificates for your organization's infrastructure and applications.

With AWS Private CA, you can create a private CA hierarchy that is managed by AWS, which allows you to issue X.509 digital certificates for use within your organization. The private CA can be used to issue certificates for internal systems, devices, applications, and services, as well as for client and server authentication.

AWS Private CA provides a highly secure, scalable, and highly available solution for managing digital certificates. You can use it to issue, renew, and revoke certificates, and to manage the entire lifecycle of your organization's digital certificates.

Some of the key features of AWS Private CA include:

1. Integration with AWS services: 

    AWS Private CA integrates with other AWS services such as Amazon S3, AWS Certificate Manager, AWS Key Management Service (KMS), AWS CloudTrail, and AWS Identity and Access Management (IAM).

2. Automation: 

    AWS Private CA can automate the certificate issuance process, making it easy to manage large-scale deployments of certificates.

3. Security: 

    AWS Private CA is highly secure, using encryption to protect your private keys and providing options for revocation and renewal of certificates.

4. Scalability: 

    AWS Private CA is highly scalable and can support a large number of certificates for your organization.

5. Compliance: 

    AWS Private CA supports industry-standard security protocols and compliance frameworks such as HIPAA, PCI DSS, and SOC.


Overall, AWS Private CA provides a highly secure and scalable solution for managing digital certificates for your organization, enabling you to issue and manage certificates for your internal systems, devices, applications, and services.


### **Certificate broker and root certificate authority**


A **certificate broker** and a **root certificate authority** (CA) are both involved in managing digital certificates in a public key infrastructure (PKI) system, but they have different roles and responsibilities.


A **root CA** is the top-level CA in a PKI hierarchy, responsible for issuing and managing root certificates. Root certificates are used to establish trust for digital certificates issued by lower-level CAs, also known as subordinate CAs. Root certificates are typically distributed to devices and web browsers to enable them to trust the digital certificates issued by the subordinate CAs.


A certificate broker, on the other hand, is a service that provides a central point of control for the issuance and management of digital certificates. It acts as an intermediary between clients and CAs, handling the communication and management of certificates on behalf of the clients. Certificate brokers can issue certificates from multiple CAs and provide additional services such as certificate revocation and renewal.

In summary, a root CA is responsible for issuing and managing root certificates, which establish trust for digital certificates issued by subordinate CAs. A certificate broker is an intermediary service that manages the issuance and management of digital certificates on behalf of clients, providing a centralized point of control and additional services such as certificate revocation and renewal.

### **Operational and Subordinate certificates**


Operational certificates and subordinate certificates serve different purposes within a Public Key Infrastructure (PKI). Here are the key differences between them:


**Operational Certificates:**

1. Purpose: 

    Operational certificates are issued to servers, devices, or individuals to authenticate, secure communications, and establish encrypted connections. They are used for operations like SSL/TLS connections, code signing, email encryption, and document signing.

2. Trust level: 

    Operational certificates are issued by an intermediate or root Certificate Authority (CA) and are trusted based on the chain of trust established by the PKI hierarchy.

3. Validity period: 

    The validity period of operational certificates is typically shorter, ranging from months to a few years. This is because operational certificates are more likely to be exposed to security threats, so shorter validity periods help mitigate risks.

4. Revocation: 

    If an operational certificate is compromised or the certificate's private key is lost, it can be revoked by the issuing CA, and a new certificate will be issued.

5. Scope: 

    Operational certificates are usually limited to specific domains, devices, or individuals.

**Subordinate Certificates:**

1. Purpose: 

    Subordinate certificates are issued to intermediate CAs, allowing them to issue operational certificates on behalf of the root CA. This reduces the risk and workload of the root CA, as it does not need to directly issue all operational certificates.

2. Trust level: 

    Subordinate certificates are issued by a root CA or another intermediate CA higher in the PKI hierarchy. They are used to establish the chain of trust between the root CA and the operational certificates issued by the intermediate CA.

3. Validity period: 

    Subordinate certificates usually have longer validity periods compared to operational certificates, as they represent the trust in an intermediate CA rather than an individual device or service.

4. Revocation: 

    Revoking a subordinate certificate is a significant event, as it impacts all operational certificates issued by the affected intermediate CA. This is typically done in case of a compromise or if the intermediate CA's private key is lost.

5. Scope: 

    Subordinate certificates are not limited to specific domains, devices, or individuals. Instead, they authorize an intermediate CA to issue operational certificates within the scope defined by the root CA.

In the case of AWS Private CA, a subordinate certificate allows AWS to act as an intermediate CA and issue operational certificates on behalf of the root CA. This enables customers to create and manage their own private PKI hierarchy within AWS, ensuring secure communications within their cloud infrastructure.


**mTLS** stands for Mutual Transport Layer Security, which is a type of secure communication protocol used to authenticate and encrypt data transmission between two parties over a network. In mTLS, both the client and the server are required to present digital certificates to each other to verify their identities and establish a secure connection.


When a client initiates a connection to a server using mTLS, it presents its own digital certificate to the server as proof of its identity. The server then verifies the authenticity of the client's certificate by checking its digital signature against a trusted certificate authority (CA) or a certificate revocation list (CRL).


**MQTT** : 



## **III. IoT Core for raw Data Ingestion**

_IoT Core centralizes the datas coming from the vehiclesas it is designed to support billions of devices and trillions of messages while ensuring low latency and high throughput. Device authorization and authentification is ensured, communication is established through the MQTT protocol and easily integrates and pass the datas to our kinesis firehorse service, our kinesis stream and IoT Defender._

[Next-Gen Vehicle Communication with AWS Services](https://docs.aws.amazon.com/whitepapers/latest/designing-next-generation-vehicle-communication-aws-iot/designing-next-generation-vehicle-communication-aws-iot.html)

—------------------

### **Fleetwise vs IoTCore vs Greengrass : Differences and Complementarity**

Greengrass is an edge runtime supporting cloud service. You can build your device software on top of the Greengrass edge runtime. However, AWS IoT FleetWise is purpose-built to collect and transform. 

Transfer vehicle data to the cloud in near real-time if you wish to do edge ML - Greengrass can be an excellent addition to run along with FleetWise, make inferences, then sends vehicle data to the cloud - if you only want to send telematics from the car to the cloud FleetWise can handle that for you, it has the libraries for most cars to understand the CAN data (let's say fluid temperatures or battery voltage) and send it to AWS - utilizing IoT core you can directly send that data to databases such as Timestream. 

The AWS IoT FleetWise Edge Agent running in your vehicle's GoldBox uses data collection schemes to control what data to collect and when to transfer it to the cloud. Data collected and ingested through AWS IoT FleetWise Edge Agent software can go directly into your Amazon Timestream table via AWS IoT Core. 
AWS IoT FleetWise integrates with AWS IoT Core to support secure communication between the Edge Agent software and the cloud through MQTT. Each vehicle corresponds to an AWS IoT thing. You can use an existing AWS IoT thing to create a vehicle or set AWS IoT FleetWise to create an AWS IoT thing for your vehicle automatically. 
AWS IoT Core supports authentication and authorization that help securely control access to AWS IoT FleetWise resources. Vehicles can use X.509 certificates to get authenticated (signed in) to use AWS IoT FleetWise, and AWS IoT Core policies to get authorized (have permissions) to perform specified actions.

[Fleetwise/IoTCore/Greengrass](https://repost.aws/questions/QUUvA2qlnsTTS3Kjerl6ErFg/fleetwise-vs-iot-core-vs-greengrass-goldvip)

### **AWS IoT Core**

is a managed cloud service that enables secure and reliable communication between Internet of Things (IoT) devices and cloud applications. It is designed to support billions of devices and trillions of messages while ensuring low latency and high throughput. Here's an exhaustive description of IoT Core and its advantages when dealing with thousands of connected vehicles sending terabytes of data per hour:

1. 	Device connectivity: 

    IoT Core supports MQTT, WebSockets, and HTTP/2 protocols, enabling seamless connectivity between various IoT devices, including connected vehicles, and the cloud.

2. 	Device authentication and authorization: 

    IoT Core provides robust authentication and authorization mechanisms, including X.509 certificate-based, token-based, and custom authorizers. This ensures secure communication between connected vehicles and the cloud.

3. 	Device registry and management: 

    IoT Core allows you to create and manage a registry of devices, providing metadata and indexing capabilities for better fleet management of connected vehicles.

4. 	Device shadow: 

    IoT Core maintains a virtual representation (shadow) of each connected device, allowing you to track and synchronize the device's state even when it's offline. This is particularly useful for managing vehicle fleets with intermittent connectivity.

5. 	Rules engine: 

    IoT Core features a rules engine that processes and routes data to other AWS services based on user-defined rules. You can use this to transform and process terabytes of data received from connected vehicles, trigger alerts, or store data in databases.

6. 	Scalability and performance: 

    IoT Core is designed to handle billions of devices and trillions of messages, ensuring low latency and high throughput. This makes it an ideal solution for managing thousands of connected vehicles sending terabytes of data per hour.

7. 	Integration with other AWS services: 

    IoT Core integrates with other AWS services like Amazon Kinesis, AWS Lambda, Amazon S3, and Amazon DynamoDB, allowing you to process, analyze, and store the data collected from connected vehicles in a scalable and cost-effective manner.

8. 	Security and compliance: 

    IoT Core is built on AWS's secure and compliant infrastructure, ensuring data privacy and meeting industry standards like ISO 27001, ISO 27017, ISO 27018, and SOC.

9. 	Pay-as-you-go pricing: 

    IoT Core has a pay-as-you-go pricing model, allowing you to pay only for the number of messages sent and received, with no upfront costs.

Advantages in the context of thousands of IoT-connected vehicles sending terabytes of data per hour:

1. Real-time data processing: 

    IoT Core can process and route terabytes of data from connected vehicles to appropriate AWS services in real-time, enabling faster decision-making and better fleet management.

2. Improved operational efficiency: 

    IoT Core's device management, rules engine, and device shadows enable better monitoring and control of connected vehicles, improving operational efficiency and reducing costs.

3. Enhanced security: 

    Secure authentication and authorization mechanisms ensure that the data exchange between connected vehicles and the cloud is protected from unauthorized access and tampering.

4. Scalability: 

    IoT Core can handle the massive volume of data generated by thousands of connected vehicles without compromising performance or reliability.

5. Seamless integration: 

    Easy integration with other AWS services enables a wide range of use cases, from real-time analytics and machine learning to long-term storage and archiving.

6. Cost-effective: 

    Pay-as-you-go pricing ensures that you only pay for the services you use, reducing the overall cost of managing a large fleet of connected vehicles.


### **IOT Core vs Lambda**

AWS IoT Core is designed specifically for IoT use cases, and provides a number of features and capabilities that make it well-suited for handling high volumes of data from large fleets of connected devices.

One of the key benefits of using IoT Core for handling IoT data is its ability to handle large volumes of data with low latency. IoT Core provides scalable message ingestion and data processing, enabling you to handle large volumes of data from your fleet of connected devices in real-time. IoT Core also provides support for a wide range of IoT protocols, including MQTT, HTTPS, and WebSocket, making it easy to connect and manage large fleets of devices.

Another key benefit of using IoT Core is its integration with other AWS services, such as Kinesis, S3, and Lambda. This enables you to build scalable and flexible IoT solutions using a combination of different AWS services, depending on your specific requirements.

While Lambda can also be used to handle IoT data, it may not be the best choice for handling high volumes of data from large fleets of connected devices. Lambda functions are designed to be stateless and short-lived, making them better suited for handling small to medium volumes of data in short bursts. In contrast, IoT Core is designed to handle large volumes of data over longer periods of time, making it better suited for handling high volumes of data from large fleets of connected devices.


### **AWS IoT Device Management**

provides the ability to onboard, configure, and manage IoT devices at scale. It allows you to organize your devices into groups, manage device configurations, and perform firmware over-the-air (OTA) updates. In our use case, we can use IoT Device Management to register your connected vehicles as devices, group them by fleet or other criteria, and manage their configurations and OTA updates. For example, you can remotely update the firmware of all the devices in a particular fleet to ensure that they have the latest software patches and security updates.


### **IoT Defender**


### **MQTT vs HTTPS Protocols** 

(Message Queuing Telemetry Transport) topic. MQTT is a lightweight messaging protocol often used for data transmission in IoT scenarios due to its efficiency and its ability to work in constrained networks.

A topic in MQTT is like a message queue or channel that devices and applications can publish messages to or subscribe from. They essentially define the routing of messages. Topics are designed hierarchically with '/' as a delimiter. For instance, 'car/sensor/gps' could be a topic where a car's GPS sensor publishes its data.

So when you are setting up the rule in AWS IoT Core, the 'topic' in the SQL statement refers to the MQTT topic from which the data will be selected. When a device publishes a message to that topic, the rule will apply the SQL statement to the message. If the message passes the SQL statement (i.e., it matches the SELECT criteria), then the rule's action will be performed on that message.

[AWS IoT SDK and Basic MQTT Functions](https://github.com/aws/aws-iot-device-sdk-python)
[Implementation AWS MQTT](https://iotatlas.net/en/implementations/aws/)

## **IV. Datalake/Warehouse creation and Batch Analytics**

_The raw datas coming from the IoT Core are passed to Kinesis Firehorse in order to be stored to a primary raw data S3 bucket in csv. format if needed. Then the raw datas can eventually be processed and refined with EMR and stored for long term usage and query to Redshift which gives data scientist flexibility to analyze and process the very raw datas and be extracted as batch with vehicle informations._  


### **Possible improvements**

1. AWS Glue DataBrew: 

    Instead of using AWS Glue for data processing and refining, you can use AWS Glue DataBrew. DataBrew is a visual data preparation tool that allows you to clean and transform data without writing code. It can help simplify the data processing step for users who are not comfortable with writing ETL scripts in AWS Glue.

2. AWS IoT Analytics: 

    If you're looking for a more integrated solution specifically for IoT data, you can use AWS IoT Analytics. This service enables you to collect, process, enrich, and analyze IoT data. You can replace the combination of IoT Core rules, Kinesis Data Firehose, and AWS Glue with AWS IoT Analytics for a more streamlined solution. IoT Analytics can store processed data in Amazon S3 and can be integrated with Amazon Redshift for further analysis.

3. Redshift Spectrum: 

    If you're using Amazon Redshift for data analysis, consider using Redshift Spectrum to query data directly from S3 without the need to load it into Redshift. This can save time and resources spent on loading data into the Redshift cluster, especially if you're only querying a subset of the data. With Redshift Spectrum, you can keep your data in S3 and run SQL queries on it directly from your Redshift cluster.

—---------------- 


### **Data Warehouse vs DataLake**

 Data warehouses and data lakes are both designed for storing and managing large volumes of data, but they serve different purposes and have unique characteristics. Here are the key differences between them:


1. Data type and structure:

    * Data Warehouse: 

        Stores structured data in a schema, typically using a relational database management system (RDBMS). Data is transformed and cleaned before being loaded into the warehouse, making it suitable for analysis and reporting.

    * Data Lake:

        Stores data in its raw, native format, including structured, semi-structured, and unstructured data. This allows for greater flexibility in the types of data that can be ingested and stored.

2. Data processing:

    * Data Warehouse: 

        Follows the ETL (Extract, Transform, Load) process, where data is extracted from source systems, transformed into a common format, and loaded into the warehouse.

    * Data Lake: 

        Follows the ELT (Extract, Load, Transform) process, where data is first loaded in its raw format and then transformed as needed during analysis.

3. Schema design:

    * Data Warehouse: 

        Uses a schema-on-write approach, where data is structured into a predefined schema before being loaded. This ensures that the data is consistent and ready for analysis.

    * Data Lake: 

        Uses a schema-on-read approach, where data is stored in its raw format and a schema is applied only when it's read for analysis. This allows for more flexibility and adaptability to changing business requirements.

4. Storage:

    * Data Warehouse: 
        
        Typically built on traditional RDBMS technologies, which store data in tables and use a columnar or row-based storage format.

    * Data Lake: 

        Built on distributed storage systems like Hadoop Distributed File System (HDFS) or cloud-based object storage like Amazon S3, which can store large volumes of data at a lower cost than traditional RDBMS.

5. Querying and analysis:

    * Data Warehouse:

        Optimized for fast, complex queries and aggregations on structured data. Supports SQL and other query languages for data analysis and reporting.

    * Data Lake: 

        Supports a variety of data processing and analytics tools, including batch processing, real-time processing, machine learning, and advanced analytics. Requires additional tools and services to query and analyze the data.

6. Use cases:

    * Data Warehouse: 

        Ideal for structured data analysis, reporting, and business intelligence use cases, where data consistency and query performance are critical.

    * Data Lake: 

        Suited for a broader range of use cases, including big data processing, advanced analytics, machine learning, and artificial intelligence, as well as handling diverse and evolving data sources.

In summary, data warehouses are optimized for storing and analyzing structured data, while data lakes are designed to store and process diverse data types at scale. The choice between a data warehouse and a data lake depends on your specific data storage, processing, and analysis requirements.

### **Batch Data Pipeline**


1. 	**IoT Fleetwise**: 

    Collects data from your fleet of vehicles and securely sends it to AWS IoT Core.

2. 	**IoT Core**: 

    Processes and routes the data to the appropriate AWS service, in this case, Amazon Kinesis Data Firehose.

3. 	**Kinesis Data Firehose**: 

    Ingests, buffers, and delivers the data to Amazon S3. You can configure Kinesis Data Firehose to convert the incoming data format (e.g., JSON) into CSV before storing it in S3.

4. 	**S3**: 

    Stores the CSV-formatted data from Kinesis Data Firehose, making it available for further processing by Amazon EMR.

5. 	**EMR**: 

    Amazon EMR can be used to process the data in S3 using distributed data processing frameworks like Apache Spark or Apache Hadoop. You can write a custom application to process time series data, calculate features, and aggregate results into batch CSV files. Additionally, you can generate the XML file describing the data schema as part of this process.

6. 	**Redshift**: 

    If you want to store the processed data in Amazon Redshift, you can load the CSV files generated by the EMR processing step into the data warehouse for further analysis, querying, or reporting.

If we only need to store the processed data as CSV files in S3 and do not require the data warehouse capabilities of Amazon Redshift, you can skip the Redshift step. Instead, write the processed CSV files and XML schema file back to S3 after processing with EMR. This will make the processed data available for further analysis or consumption by other applications.


### **Kinesis Data Streams et Kinesis Data Firehose**

**Kinesis Data Streams** is a real-time data streaming service that allows continuous data ingestion, data persistence for 24 hours or more, data processing using Lambda or Amazon Kinesis Client Library, and data replication across multiple AWS regions. Kinesis Data Streams also enables real-time data processing using tools such as Apache Storm, Apache Flink, or Spark Streaming. It is primarily used for use cases requiring low-latency data processing, such as clickstream analysis, fraud detection, or real-time alerts.

**Kinesis Data Firehose**, on the other hand, is a real-time data delivery streaming service that allows routing data to destinations such as Amazon S3, Amazon Redshift, Amazon Elasticsearch, or Amazon Kinesis Data Analytics. Kinesis Data Firehose also performs real-time data transformations using Lambda or third-party software and supports data replication across multiple AWS regions. Kinesis Data Firehose is primarily used for use cases that require simplified real-time data processing, such as real-time data archiving or real-time log analysis.

In summary, Kinesis Data Streams is a real-time data streaming service for the ingestion, processing, and persistence of continuous data, while Kinesis Data Firehose is a real-time data delivery streaming service for routing and transforming data to storage or real-time analysis destinations.

### **Glue vs Lambda to perform final data cleaning job** 

**AWS Glue** is a fully managed extract, transform, and load (ETL) service that can automate the process of converting the processed data to a CSV format. It can also generate metadata and data catalog, which makes it easier to discover and query the data later. Glue can also handle schema evolution and can adapt to changes in the schema over time.

On the other hand, AWS Lambda is a serverless compute service that can be used to execute custom code. It can also be used to convert the processed data to a CSV format and generate the metadata XML file. **Lambda** is a good choice if you need more flexibility in terms of the code that needs to be executed, and if you have smaller volumes of data to process.

In terms of efficiency, **Glue** is a better choice for larger volumes of data as it can scale horizontally and process data in parallel. It also provides features such as data partitioning, which can further optimize the processing time. However, if you have smaller volumes of data, **Lambda** can be a more cost-effective solution.

Another option to consider is using a combination of both Glue and Lambda. For example, you could use Glue for the initial conversion of the data to CSV, and then use Lambda to perform additional processing on the data, such as generating the metadata XML file. 


### **Glue vs EMR batch/real-time processing**

Glue and EMR are two AWS services that can be used to process and analyze data. Each has its own advantages and disadvantages depending on your use case. For your real-time processing and analysis use case for a fleet of automotive sensors in the cloud, here is a comparison between AWS Glue and AWS EMR:

**AWS Glue**:

Fully managed Extract, Transform, and Load (ETL) service. Does not require cluster management or infrastructure configuration.
Billing based on runtime and resources used (pay-as-you-go). Native support for integration with other AWS services such as S3, Redshift, RDS, etc. Ability to create ETL tasks based on Apache Spark and Python. Integrated metadata and data catalog management.
Better suited for batch workloads and ETL tasks.

**AWS EMR (Elastic MapReduce)**:

Fully managed data processing service based on Hadoop and Spark.
More control over cluster configuration and management.
Billing based on EC2 instances used for the cluster and their usage duration.
Support for various data processing frameworks such as Hadoop, Spark, Flink, etc.
Ability to run tasks based on various programming languages (Scala, Java, Python, etc.).
Better suited for real-time workloads, interactive analytics, and complex data processing tasks.
For our specific use case, here are some considerations:
If you prefer a fully managed service with minimal configuration and tight integration with other AWS services, AWS Glue might be a better choice. If you need real-time analysis and complex data processing with more granular control over cluster management, AWS EMR might be better suited.

Overall, it is important to evaluate your performance, cost, and resource management requirements before choosing between AWS Glue and AWS EMR for your real-time processing and analysis use case for a fleet of automotive sensors in the cloud.

### **Aurora vs Redshift for Data Warehousing**

[Aurora vs Redshift](https://hevodata.com/blog/amazon-rds-to-redshift-etl/) 

**Amazon Aurora:**

Is a relational database service compatible with MySQL and PostgreSQL.
Suitable for transactional workloads (OLTP) with complex queries and frequent read and write operations.
Offers high availability, automated backups, and replicas for better read performance.
In your IoT use case, Aurora can be used to store and manage real-time data from your sensors and connected devices.

**Amazon Redshift:**

Is a data warehouse service designed to handle large amounts of structured and semi-structured data.
Optimized for analytical workloads (OLAP) with aggregation and reporting queries on large datasets.
Integrates seamlessly with other AWS services such as S3, Kinesis, and data visualization tools like QuickSight.
In your IoT use case, Redshift can be used to store, analyze, and extract insights from historical data of your vehicle fleet, providing reports and analyses on trends, performance, and patterns.

For our IoT vehicle fleet use case, it may be best to use a combination of Aurora and Redshift. Aurora can be used to manage real-time data and transactional operations, while Redshift can be used to store and analyze historical data for analysis and reporting purposes.

## **V. Real-time Analytics**

_For real-time analysis, processing and visualization of our vehicle fleets sensors behaviors, the data coming from IoT Core is send to a kinesis streams which then forward the data to Kinesis Analytics with which we can perform real-time data processing with Analytic Studio. Data is then stored to a Timestream Database connected to Quicksight for real-time fleet visualization._

[Tuto : set up streaming etl pipelines with kinesis data analytics](https://aws.amazon.com/getting-started/hands-on/set-up-streaming-etl-pipelines-apache-flink-and-amazon-kinesis-data-analytics/?ref=gsrchandson)

### **Advantages of this pipeline**

1. Direct visualization: 

    The pipeline connects Amazon Timestream to Amazon QuickSight (or Grafana) for real-time visualization, which is more straightforward than using Kinesis Data Firehose and Amazon S3. This makes it easier to create visualizations and dashboards for real-time monitoring of your fleet.

2. Simplified data flow: 

    The pipeline eliminates the need for Kinesis Data Firehose and Amazon S3, simplifying the data flow and reducing the number of services involved. This can make the pipeline easier to manage and monitor.

3. Optimized for time-series data: 

    Storing processed data in Amazon Timestream is more optimized for time-series data, which is the focus of our use case. Amazon Timestream provides efficient storage and fast querying capabilities for time-series data, making it suitable for the scenario.

### **Potential Improvements**

1. AWS IoT Analytics: 

    You can use AWS IoT Analytics in combination with AWS IoT Core for a more integrated solution to collect, process, and analyze IoT data. This service can preprocess and filter the incoming data before sending it to Kinesis Data Streams. This allows you to reduce the volume of data being processed downstream and improve the overall efficiency of your pipeline.

2. Lambda for data transformation: 

    If you need to perform complex data transformations or generate metadata schema (like XML files) that might not be easily achievable using Kinesis Data Analytics alone, you can introduce AWS Lambda in your pipeline. You can use Lambda to process the data coming from Kinesis Data Streams, apply any necessary transformations, and then send the processed data to Kinesis Data Analytics for further processing.

3. Monitoring and alerting: 

    To ensure the reliability and performance of your pipeline, consider implementing monitoring and alerting using Amazon CloudWatch. You can create custom metrics and alarms to track the health of your pipeline components and receive notifications when certain thresholds are exceeded.

4. Data retention policy: 

    Since you are using Amazon Timestream for storing processed data, it's essential to define a data retention policy to manage the storage cost and performance. Consider how long you need to retain data at different granularities and configure Timestream accordingly.

5. Optimize Kinesis Data Streams: 

    Depending on the volume and velocity of your incoming data, you might need to optimize your Kinesis Data Streams by adjusting the number of shards. Monitor the stream's performance and adjust the shard count as needed to ensure optimal throughput and performance.

6. AWS WAF and API Gateway: 

    If you're exposing APIs to interact with your data pipeline, consider implementing AWS WAF (Web Application Firewall) and API Gateway to secure and protect your API endpoints. This will help you manage access control, rate limiting, and protection against common web exploits.

7. Auto Scaling: 

    For components in your pipeline that support auto scaling (e.g., Kinesis Data Analytics, AWS Lambda), configure auto scaling policies to ensure your pipeline can handle sudden spikes in data volume or processing requirements without manual intervention.

8. Cost optimization: 

    Continuously review and optimize your pipeline's cost by monitoring AWS Cost Explorer and following best practices for each service. For example, review Amazon Timestream's pricing model and adjust data retention policies or use reserved capacity for better cost management.

9. Data encryption: 
    
    Ensure your data is encrypted both in transit and at rest. Enable server-side encryption for Amazon Timestream and Amazon S3. Use AWS Key Management Service (KMS) to manage encryption keys for better security and compliance.

10. Automated backups: 

    Implement regular automated backups for critical components in your pipeline, like Amazon Timestream or Amazon S3, to ensure data durability and facilitate disaster recovery.

11. Logging and auditing: 

    Enable logging and auditing for all components in your pipeline using AWS CloudTrail and Amazon CloudWatch Logs. Regularly review logs and audit trails to monitor activity, identify potential issues, and maintain compliance with security requirements.

**KinesisStream vs Kafka**

[KinesisStream vs Kafka](https://vitalflux.com/amazon-kinesis-vs-kafka-concepts-differences/)

### **IoT Analytics vs Kinesis Analytics**

[Query Your Data Streams in Real Time With Kinesis Data Analytics Studio](https://www.youtube.com/watch?v=SX_6x_wXIfA) 

[iot-analytics FAQ](https://www.amazonaws.cn/en/iot-analytics/faq/) 

AWS IoT Analytics and Kinesis Analytics are two different services that can be used for processing and analyzing data from IoT devices.

The added value of using AWS IoT Analytics over Kinesis Analytics depends on the specific use case and requirements. Here are some potential advantages of using **AWS IoT Analytics**:


1. Integration with AWS IoT Core: 

    AWS IoT Analytics is designed to integrate seamlessly with AWS IoT Core, the managed cloud service for IoT devices. This integration allows for easy ingestion of data from IoT devices, and makes it possible to enrich, filter, and transform the data using SQL-based queries.

2.  Pre-built analytics components: 

    AWS IoT Analytics includes pre-built components such as filtering, transformation, and data enrichment that can be used to process IoT data. These components can help to reduce the amount of custom code that needs to be written and simplify the process of building analytics pipelines.

3. Data retention: 

    AWS IoT Analytics includes a built-in data retention policy that can be used to specify how long data should be retained. This can help to manage storage costs and simplify data lifecycle management.

4. Integration with other AWS services: 

    AWS IoT Analytics integrates with other AWS services such as S3, Kinesis, and Lambda, which can be used to store and process data.

Overall, AWS IoT Analytics provides a fully managed and integrated solution for processing and analyzing data from IoT devices, with pre-built analytics components, data retention policies, and integration with other AWS services. It can be a good choice for organizations that require an end-to-end solution for IoT data processing and analysis. However, if real-time analytics is a critical requirement, then Kinesis Analytics may be a better choice, as it is designed specifically for real-time streaming data processing and analysis.


### **IoT Events vs IoT Analytics** 

Both services are provided by AWS to help users to manage and analyze IoT data. However, they have different use cases and functionalities.

IoT Events is a service that helps users to detect and respond to events from their IoT devices in near real-time. It allows you to create rules that match incoming data from your devices and trigger actions based on those rules. IoT Events is mainly used for reactive event processing, anomaly detection, and sending notifications to users or other systems. Here are two use cases for **IoT Events**:

1. Predictive maintenance: 

    Imagine you have a fleet of vehicles, and you want to detect when one of your vehicles has an engine problem before it breaks down. With IoT Events, you can create rules that monitor the data from the vehicle sensors and detect anomalies. For example, if the engine temperature exceeds a certain threshold, you can trigger an event that alerts your maintenance team to take action.

2. Security monitoring: 

    You can use IoT Events to monitor your IoT devices for security breaches. For example, you can create a rule that triggers an event when an IoT device tries to connect to an unauthorized network. The event can then be used to send notifications to security teams, block the device's access, or initiate other actions.

On the other hand, IoT Analytics is a service that allows you to process and analyze large volumes of IoT data. It provides advanced data processing, querying, and visualization capabilities, enabling users to gain insights into their IoT data. Here are two use cases for **IoT Analytics**:

1. Predictive maintenance: 

    Similar to IoT Events, you can use IoT Analytics to monitor your IoT devices for potential maintenance issues. However, with IoT Analytics, you can perform more advanced data processing and analysis. For example, you can use machine learning algorithms to detect patterns in your data that may indicate an impending failure. You can also perform root cause analysis to identify the underlying issues causing the failures.

2. Supply chain optimization: 

    Suppose you have a warehouse that receives a large number of shipments each day. With IoT Analytics, you can analyze the data from your sensors and optimize your supply chain operations. For example, you can use historical data to predict the arrival times of your shipments, identify bottlenecks in your warehouse, and optimize your inventory levels.

In summary, IoT Events is best suited for reactive event processing, anomaly detection, and notifications, while IoT Analytics is better for data processing, analysis, and visualization. Both services can be used together to provide a complete IoT solution.

### **Data Path Kinesis Analytics to Kinesis stream or vise versa ?**

data can be sent from Kinesis Streams to Kinesis Analytics and vice versa.

The best way to send data from Kinesis Streams to Kinesis Analytics is to create a Kinesis Analytics application, and then configure the input stream to point to the Kinesis stream where the data is being sent. The Kinesis Analytics application can then use SQL queries to process the data in real-time and generate output data.

To send data from Kinesis Analytics to Kinesis Streams, you can configure the Kinesis Analytics application to generate a stream output. The output stream can be configured to write data to a Kinesis stream or another destination such as S3 or Redshift.


### **Timestream** 

Timestream is a fully managed time-series database service that is designed to handle large volumes of time-series data with high accuracy and precision.

Timestream provides built-in functions for calculating time-series derivatives such as moving averages, and supports standard SQL queries for data retrieval and analysis. Timestream also provides built-in support for storing and querying metadata, such as tags and custom attributes.

One potential advantage of using Timestream over other databases is its ability to handle high write and query volumes with low latency. Timestream is designed to scale automatically to handle changing data volumes and query patterns, and can support millions of write requests per second.


### **Timestream vs MemoryDB for Redis and Elasticache**

Amazon MemoryDB for Redis and Amazon ElastiCache are in-memory data store services that are often used for caching, session storage, pub/sub messaging, and real-time analytics. They offer high performance, scalability, and simplicity, but may not be the best fit for every use case. Here are some potential drawbacks in the context of processing time series data from a fleet of IoT vehicles.

1. Data Durability and Persistence: 

    While both MemoryDB and ElastiCache support data persistence, they are primarily in-memory data stores designed for speed rather than durability. Data is stored in memory and can be lost in the event of a failure if not properly configured for persistence. In contrast, a database service like Amazon Timestream is designed for time series data and provides automatic data replication across multiple Availability Zones for durability.

2. Cost: 

    In-memory data stores can be more expensive than disk-based databases due to the cost of memory. The cost can increase significantly if you need to store large volumes of time series data.

3. Data Analysis and Querying: 

    MemoryDB and ElastiCache do not natively support complex querying capabilities. They are key-value stores and are not designed for complex data analysis tasks. On the other hand, time series databases like Timestream provide built-in tools for time series data analysis.

4. Scalability: 

    While MemoryDB and ElastiCache are scalable, managing the scaling process can be complex and resource-intensive. They can scale vertically (by adding more memory to an existing node) and horizontally (by adding more nodes), but both processes can involve manual intervention and potential downtime. In contrast, some other AWS services like Timestream are designed to handle scalability automatically.

5. Data Retention and Archiving: 

    MemoryDB and ElastiCache are not designed for long-term data storage and archiving. If you need to keep your time series data for a long period of time for compliance or analysis, a time series database or data warehouse might be a better fit.

For a fleet of IoT vehicles sending time series data, you might use a combination of different data stores: for example, MemoryDB or ElastiCache for real-time processing and analysis, and Timestream or another database for longer-term storage and analysis.

### **Graphana vs Quicksight**

Kinesis Analytics can be integrated with both QuickSight and Grafana for data visualization, and the choice of tool depends on your specific use case and requirements.

QuickSight is a fully managed business intelligence (BI) service that allows you to create interactive dashboards and reports. It provides a drag-and-drop interface for creating visualizations, and supports a wide range of data sources including Kinesis Analytics. QuickSight also provides advanced features such as machine learning-powered insights and natural language queries. QuickSight is a fully managed business intelligence service that is optimized for integration with other AWS services, including Timestream. QuickSight provides built-in integration with Timestream, which enables you to easily connect to and visualize your time-series data in real-time. 

Grafana is an open-source platform for data visualization and monitoring. It provides a highly customizable and flexible dashboarding framework, and supports a wide range of data sources including Kinesis Analytics. Grafana also provides features such as alerting, data exploration, and collaboration tools.


## **VI. Machine Learning Pipeline**

_The process datas with the time serie features is processed by the ETL (Extract Transform and Load) service AWS Glue in order to be cleaned, converted to csv and xml format and stored to a S3 bucket that will be acessed to the datascientists. The processed datas can finally be used in Sagemaker which allows you to load, processed, train the final data with a model, and host the model with AWS native machine learning tools._


### **Amazon EMR** 

Managed cluster platform that simplifies running big data frameworks, such as Apache Hadoop and Apache Spark, on AWS to process and analyze vast amounts of data. By using these frameworks and related open-source projects, such as Apache Hive and Apache Pig, you can process data for analytics purposes and business intelligence workloads. Additionally, you can use Amazon EMR to transform and move large amounts of data into and out of other AWS data stores and databases.

Amazon Redshift is the most widely used cloud data warehouse. It makes it fast, simple and cost-effective to analyze all your data using standard SQL and your existing Business Intelligence (BI) tools. It allows you to run complex analytic queries against terabytes to petabytes of structured and semi-structured data, using sophisticated query optimization, columnar storage on high-performance storage, and massively parallel query execution.

Amazon EMR (Elastic MapReduce) on AWS can be used to transform raw JSON files stored in AWS to structured data in CSV format with XML metadata descriptions. EMR is a fully-managed cloud-based big data processing platform that allows users to process large amounts of data using Apache Hadoop, Spark, or other big data processing frameworks.

To transform raw JSON files to CSV with XML metadata, you can use the following steps:


1. Define an XML schema to describe the structure of the JSON data. The schema should define the fields in the JSON data, their types, and any nested or repeated structures.

2. Use Amazon S3 to store the JSON data files.

3. Create an EMR cluster with the appropriate configuration, including the Hadoop, Spark, or other processing framework of your choice.

4. Use a tool like Apache Hive or Apache Pig to read the JSON data files and convert them to structured data in CSV format with the XML metadata.

**Amazon Sagemaker**

Amazon SageMaker is a fully managed service that enables you to build, train, and deploy machine learning models quickly. It supports several built-in algorithms and deep learning frameworks that can be used for time series analysis tasks, including anomaly detection and prediction. Here are some of the Amazon SageMaker models and algorithms that are well-suited for time series data:


1. DeepAR: 

    DeepAR is a built-in algorithm in SageMaker that uses recurrent neural networks (RNNs) for time series forecasting. It is designed to handle multiple related time series and can learn to capture complex patterns, trends, and seasonality in the data. DeepAR is suitable for various time series prediction tasks, such as sales forecasting, resource planning, and energy demand prediction.

2. Prophet: 

    Although not a built-in algorithm, you can use Facebook's Prophet library with Amazon SageMaker by creating a custom container. Prophet is a powerful time series forecasting tool that is particularly effective at handling data with strong seasonal effects and missing data. It can be used for a wide range of time series prediction tasks.

3. Random Cut Forest (RCF): 

    RCF is a built-in unsupervised learning algorithm in SageMaker that can be used for anomaly detection in time series data. It works by building a forest of decision trees and scoring each data point based on its distance from the tree's leaves. RCF can help you identify unusual patterns and outliers in your time series data.

4. XGBoost: 

    While XGBoost is primarily known for its use in classification and regression tasks, it can be adapted for time series forecasting by creating appropriate features from the time series data (e.g., lagged values, rolling averages, etc.). You can use the built-in XGBoost algorithm in SageMaker for time series prediction tasks.

5. Seq2Seq: 

    Sequence-to-sequence (Seq2Seq) models, based on recurrent neural networks (RNNs) or transformer architectures, can be used for time series forecasting. Although not built-in, you can use TensorFlow or PyTorch within SageMaker to implement custom Seq2Seq models for your time series prediction tasks.

6. LSTMs and GRUs: 

    Long Short-Term Memory (LSTM) and Gated Recurrent Unit (GRU) are types of recurrent neural network (RNN) architectures that can be effective for modeling time series data. You can use TensorFlow or PyTorch within SageMaker to implement custom LSTM or GRU models for time series prediction and anomaly detection tasks.

7. 1D Convolutional Neural Networks (1D CNNs): 

    1D CNNs can also be used for time series analysis, including forecasting and anomaly detection. You can use TensorFlow or PyTorch within SageMaker to implement custom 1D CNN models tailored to your specific time series tasks.

In summary, Amazon SageMaker supports several built-in algorithms and deep learning frameworks that can be used for time series analysis tasks, including DeepAR, Prophet, Random Cut Forest, XGBoost, Seq2Seq, LSTMs, GRUs, and 1D CNNs. Depending on your specific requirements, you can choose the most appropriate model or algorithm for your time series prediction or anomaly detection tasks.


## **VII. Sensor Log Analysis and Security  Pipeline**

_AWSIoT Device Defender audits IoT devices, detects anomalies, and alerts via Amazon SNS. AWS CloudWatch monitors resources, logs events, and triggers AWS Lambda for certificate rotation tasks. GuardDuty and CloudTrail monitor malicious activities, recording account actions, and facilitating security findings for remediation._

**—-----------------**


1. AWS IoT Device Defender audits IoT devices, detects anomalies, and alerts via Amazon SNS. 
2. AWS CloudWatch monitors resources, logs events, and triggers AWS Lambda for certificate rotation tasks. 
3. GuardDuty and CloudTrail monitor malicious activities, recording account actions, and facilitating security findings for remediation.

### **Justifications for Implementing this Pipeline**

We need a log processing and analysis chain that can identify abnormal sensor behavior. Although this logic could be extended to all authorized AWS services, we will focus specifically on the interaction between the vehicles and the first cloud service they connect with.

Implementing this pipeline is essential due to the following reasons:

    1. Monitoring Device Performance: By scrutinizing the failure rates and connection errors among our IoT devices, we gain a deeper understanding of the operational status of our fleet. If a vehicle is transmitting too much or too little data, it might point towards an issue in the vehicle's components or the overall system. This real-time information enables us to pinpoint and resolve potential issues promptly.

    2. Identifying Security Threats: Our system expects a particular transmission frequency from each vehicle - for instance, messages every two minutes. If the frequency suddenly increases to a message every nanosecond, it suggests a significant problem, possibly indicating a security breach like an attack attempt or certificate forgery.

Incorporating services like AWS IoT Device Defender, AWS CloudWatch, GuardDuty, and CloudTrail into our pipeline enhances our ability to audit IoT devices, detect anomalies, and respond swiftly:

    1. AWS IoT Device Defender audits IoT devices, identifies anomalies, and sends alerts via Amazon SNS.

    2. AWS CloudWatch monitors resources, logs events, and triggers AWS Lambda for certificate rotation tasks.

    3. GuardDuty and CloudTrail monitor for malicious activity, record account actions, and facilitate security findings for prompt remediation.

### **IoT Defender**

AWS IoT Device Defender provides security and compliance monitoring for IoT devices. It allows you to monitor the behavior of your devices, set up rules to detect suspicious activity, and receive alerts when an anomaly is detected. In your use case, you can use IoT Device Defender to monitor the behavior of your connected vehicles and detect any anomalies that could indicate a security breach or malfunction. For example, you can set up rules to detect when a device starts sending data to a suspicious IP address or when it starts transmitting data at an unusually high frequency. When an anomaly is detected, you can receive an alert and take action to investigate and mitigate the issue.


### **Cloudwatch and Cloutrail added value**

Enable CloudTrail to log all API calls made to IoT Core by going to the AWS Management Console, selecting the CloudTrail service, and creating a new trail. In the trail settings, select the IoT Core API actions that you want to log.


1. Set up CloudWatch Alarms to monitor for specific events in your IoT Core environment. For example, you could create an alarm to notify you if there is a sudden increase in the number of failed messages from your connected vehicles.
2. Create CloudWatch Logs to store the logs generated by CloudTrail. You can configure CloudTrail to deliver the logs directly to CloudWatch Logs, which allows you to view and analyze the logs in near real-time.
3. Use CloudWatch Metrics to monitor the health of your IoT Core environment. CloudWatch Metrics provide detailed information on the performance of your IoT Core environment, such as message delivery rates, error rates, and connection rates.

## **VIII. Consumers, Users, Administrators**

_The architecture is managed and used by  three different group : The administrator who open accounts, grants permissions, IAM, roles and manage organisations. Data Scientists who have prenium and exclusive access to the overall infrastructure and the IT Cyber and auditing team which has read access to the IoT Core and Security and log analysis services. Administrator can provide the cyber team with extended access and permissions if deemed necessary._

—-------

### **IAM, Roles, Roles Based Access Control (RBCA), SIngle Sign On (SSO)**

To better manage and have a better overview of IAM users and their roles across different AWS services, you can use AWS Organizations, AWS Single Sign-On (SSO), and AWS Identity and Access Management (IAM) with Role-Based Access Control (RBAC).

AWS Organizations allows you to centrally manage and govern multiple AWS accounts. With AWS Organizations, you can create policies that automatically apply to member accounts, such as service control policies (SCPs) to restrict access to specific AWS services.

AWS Single Sign-On (SSO) enables you to centrally manage access to multiple AWS accounts and business applications. With AWS SSO, you can create user groups and assign permissions to these groups using either built-in policies or custom policies. This allows you to easily manage access for different roles and users across different AWS services.

Finally, AWS IAM with RBAC allows you to create roles with specific permissions and assign them to users or groups of users. With IAM, you can also define policies to restrict access to specific actions or resources within a service. IAM also offers the ability to set up multi-factor authentication (MFA) for added security.

By using these services together, you can create a centralized management and governance framework that allows you to easily manage IAM roles and permissions across multiple AWS accounts and services.

## **IX. Continous Integration, Continuous Delivery and Deployment (CICD)**

_This pipeline is managed by the datascientist and contains the tools necessary  for collaboration and source code storage (gitlab), deployment automation (Codepipeline) and building and testing the infrastructure (CodeBuild)._

—---------------------------

### **Codebuild, CodePipeline, Gitlab**

GitLab, CodePipeline, and CodeBuild can all be used together to create a seamless pipeline for building and deploying applications for IoT Automotive Fleet.

GitLab is a version control system that allows developers to collaborate on code and track changes. It can be used to store code for the IoT Automotive Fleet and manage branches and releases.

CodePipeline is a continuous delivery service that can be used to automate the build, test, and deployment process of the code stored in GitLab. It can also be used to create custom pipelines that automate other processes, such as testing, packaging, and deployment.

CodeBuild is a fully managed build service that compiles source code, runs tests, and produces software packages that can be deployed. It can be integrated with CodePipeline to automatically build and test code changes before deploying them to production.

1. Developers make changes to the code in GitLab.
2. CodePipeline is triggered to automatically build and test the code changes.
3. CodeBuild compiles the source code, runs tests, and produces a deployable software package.
4. CodePipeline deploys the new software package to a test environment for further testing.
5. Once the changes have been tested, CodePipeline deploys the software package to production.

how you could use GitLab, CodePipeline, and CodeBuild together in a coherent way:

1. Create a GitLab repository to store your code.
2. Set up a webhook in GitLab to trigger a CodePipeline when a new commit is pushed to the repository.
3. Configure your CodePipeline to pull the code from GitLab and run a CodeBuild project.
4. Define your CodeBuild project in a buildspec.yml file in the root of your GitLab repository. This file specifies the build commands and environment settings for CodeBuild.
5. an example of how you could integrate Gitlab with AWS CodePipeline:
    1. Create an S3 bucket:

```
aws s3api create-bucket --bucket <bucket_name> --region <region> --create-bucket-configuration LocationConstraint=<region>

```

6. This bucket will be used to store the deployment artifacts that are created by AWS CodePipeline.
7. Create an IAM role: \
Create an IAM role with the necessary permissions to access the S3 bucket and deploy the application. Assign the role to the CodePipeline service.
8. Create a CodePipeline pipeline: \
Create a pipeline in CodePipeline that will handle the code deployment process. The pipeline will have three stages: Source, Build, and Deploy. \
In the Source stage, select GitLab as the source provider and provide the repository information. \
In the Build stage, select CodeBuild as the build provider and configure the build environment to match your project. \
In the Deploy stage, select Elastic Beanstalk as the deployment provider and configure the deployment settings.
9. Create a CodeBuild project: \
Create a CodeBuild project that will build the application artifacts. Configure the project to use GitLab as the source repository and S3 as the artifact store.
10. Create a webhook in GitLab: \
Create a webhook in GitLab that will notify AWS CodePipeline when changes are made to the repository.

## **X. Durability, fault tolerance, and disaster tolerance**

To ensure durability, fault tolerance, and disaster tolerance of S3 buckets, Timestream databases, and overall IoT services infrastructure, several best practices and alternatives can be considered.

For **S3 buckets**, the following approaches can be taken:

1. Cross-Region Replication: 

    This is a feature provided by Amazon S3 to replicate objects across different AWS regions. Cross-Region Replication helps ensure that data is durable and available in the event of a disaster. By replicating the objects across different regions, you can maintain multiple copies of the data and minimize data loss. This approach provides a simple way to replicate S3 buckets across different regions with low latency and high durability.

2. Versioning: 

    Enabling versioning on S3 buckets is a way to store multiple versions of an object in the same bucket. This feature helps in case an object is accidentally deleted or modified. With versioning, you can restore the object to a previous version, which helps ensure durability and fault tolerance.

3. Lifecycle Policies: 

    This feature allows you to transition objects to different storage classes or delete objects based on predefined rules. This helps reduce the cost of storing objects in S3 and ensures that data is properly managed and retained based on its lifecycle.

For **Timestream databases**, the following approaches can be taken:

1. Cross-Region Replication: 

    This feature can also be used to replicate Timestream databases across different regions, ensuring that data is durable and available in the event of a disaster. Cross-Region Replication can be configured through AWS Management Console, AWS CLI, or SDKs.

2. Backup and Restore: 

    Timestream provides a backup and restore feature that can be used to create backups of Timestream tables and databases. Backups can be used to restore data in case of accidental deletion or modification. This approach helps ensure durability, fault tolerance, and disaster tolerance.

3. Data Retention Policies: 

    Timestream allows you to define data retention policies to automatically delete old data. This helps reduce the cost of storing data in Timestream and ensures that data is properly managed based on its lifecycle.

For overall IoT services infrastructure, the following approaches can be taken:

1. Multi-AZ Deployment: 

    Deploying your IoT services in multiple Availability Zones (AZs) helps ensure high availability and fault tolerance. Multi-AZ deployment ensures that if one AZ goes down, the service will continue to function from another AZ.

2. Disaster Recovery: 

    Disaster Recovery (DR) solutions can be used to replicate data and applications to a secondary location to ensure business continuity in the event of a disaster. AWS provides several DR solutions such as AWS Backup, AWS Site-to-Site VPN, and AWS Direct Connect.

3. Auto Scaling: 

    Auto Scaling can be used to automatically adjust the capacity of your IoT services based on the demand. This helps ensure that your services can handle sudden spikes in traffic or demand, which helps ensure high availability and fault tolerance.

In conclusion, to ensure durability, fault tolerance, and disaster tolerance of S3 buckets, Timestream databases, and overall IoT services infrastructure, various approaches can be taken. The best approach depends on the specific requirements of the use case, such as data size, frequency of updates, and access patterns. By choosing the appropriate approach, organizations can ensure that their data and services are always available, durable, and fault-tolerant.

### **Security/Cost/Permissions/Backups/FaultTolerance**

1. Cybersecurity:

    All communication is secure via PKI, and AWS Private CA is used to ensure authenticity of both clients and servers. AWS IoT Core provides device authorization and authentication.
    AWS IoT Defender provides security analytics, helping to identify potential security issues.
    AWS CloudWatch logs and AWS Lambda are used for log processing and analysis, which can help identify any security threats or breaches.
    AWS IAM is used for managing access and permissions to the resources, allowing fine-grained control over who can do what.

To further enhance cybersecurity, you could consider AWS Security Hub for a more comprehensive view of your security status across AWS accounts, services, and regions. AWS WAF (Web Application Firewall) could be used to protect your applications from common web exploits.

2. Cost Analysis/Surveillance:

    AWS provides several tools for cost management such as AWS Cost Explorer, AWS Budgets, and AWS Cost and Usage Reports.
    You could set up alerts using AWS Budgets to notify you when your usage or costs exceed the thresholds you've defined.
    AWS Trusted Advisor can provide insights on cost optimization and security.

3. Database Replication:

    AWS provides several options for database replication. For example, Amazon RDS supports Multi-AZ deployments for fault tolerance, and Read Replicas for read traffic scaling.
    For DynamoDB, you can enable global tables for multi-region, active-active replication.
    For S3, you can enable Cross-Region Replication (CRR) to replicate data between buckets in different regions.

4. Fault Tolerance of Services:

    AWS IoT Core is a managed service that is designed for high availability and redundancy.
    Kinesis, Glue, Redshift, EMR, and Sagemaker are all highly available services. Multi-AZ deployments should be used where available for high availability and fault tolerance.
    AWS Lambda functions are inherently highly available and fault-tolerant.
    AWS Route 53 is a highly available and scalable DNS service, which can route traffic to healthy endpoints and avoid single points of failure.

5. IAM/Permissions Monitoring:

    AWS IAM provides tools for managing access and permissions. IAM Access Analyzer can be used to identify resources that are shared with entities outside of your account.
    AWS CloudTrail provides event history of your AWS account activity, including actions taken through the AWS Management Console, AWS SDKs, command line tools, and other AWS services. This is useful for security analysis, resource change tracking, and compliance auditing.

To further enhance IAM/Permissions monitoring, consider AWS Organizations for centrally managing policies across multiple AWS accounts. You could also implement least privilege principles, regularly review and revoke unnecessary permissions, and use multi-factor authentication for enhanced security.

### **Deploying the infrastructure globally : step by step**

To deploy this infrastructure globally and to ensure data from vehicles in different availability zones are sent to the closest hardware, you can use a combination of AWS services such as AWS Global Accelerator, AWS Route 53, and AWS CloudFront. Here are the steps you can follow:

1. Vehicle Gateway Parameterization

    This remains the same globally. All vehicles have the IoT Fleetwise SDK and AWS IoT SDK installed on their onboard computers, and are parameterized based on their sensors and computer board specs. The devices are given an individual ID and are tagged with their model. They are also attributed to a fleet for grouping and analysis.

2. Security, Communication, and Governance

    The PKI must be established globally, and the Root Authority managed by AWS Private CA must ensure communication between IoT devices and IoT Core across all regions.

3. IoT Core for Raw Data Ingestion

    For global deployment, you will need to create AWS IoT Core endpoints in each region you wish to operate in. This ensures that vehicle data is sent to the closest hardware, reducing latency and improving performance. AWS IoT Core can then pass the data to services such as Kinesis Firehose, Kinesis Stream, and IoT Defender in the same region.

4. Datalake/Warehouse Creation and Batch Analytics

    This process remains similar globally. Raw data is stored in a primary S3 bucket in the same region where the IoT Core endpoint is located, which reduces data transfer costs and latency.

5. Real-time Analytics

    For real-time analytics, use Kinesis Streams and Kinesis Analytics in the same region where the IoT Core endpoint is located. This ensures that data doesn't have to be transferred between regions for real-time analysis.

6. Machine Learning Pipeline

    The ETL process with AWS EMR and data processing with SageMaker should be done in the same region where the data is stored. This reduces data transfer costs and latency, and ensures that the machine learning models are trained on the most recent data.

7. Sensor Logs Analysis and Security Pipeline

    Similar to the previous steps, the IoT Defender, CloudWatch Logs, Lambda functions, and S3 buckets should all be located in the same region where the data is being generated.

8. Consumers, Users, Administrators

    You can use AWS IAM to manage users and their permissions globally. IAM policies can be used to grant the necessary permissions to the different groups in each region.

9. Continuous Integration, Continuous Delivery, and Deployment

    AWS CodePipeline and CodeBuild can be used globally to manage the CI/CD pipeline. You can create separate pipelines for each region to manage the deployment of infrastructure and application code independently.

10. Routing to Closest Hardware

    You can use AWS Global Accelerator to improve the availability and performance of your applications. It provides static IP addresses that act as a fixed entry point to your application endpoints in a single or multiple AWS Regions, such as your IoT Core endpoints.
    AWS Route 53 can be used to route users to the best endpoint based on geolocation routing policy. This ensures that vehicle data is always sent to the closest IoT Core endpoint.
    You can also use AWS CloudFront with regional edge caches close to your users to speed up the delivery of your IoT data. This can be useful if you need to distribute data to users globally.
    For multi-region deployment, make sure to implement proper data replication and backup strategies to ensure data durability and availability. AWS provides services like S3 Cross-Region Replication and AWS Backup for these purposes.

**TO DO**

AWS IoT Device Management

Batch analytics / IoT Defenders and Logs bien revoir la pipeline

Déployer les modèles on edge/lambda

IoT Greengrass vs IoT Core vs AppSync

Protocoles de communication

Bien revoir GLUE / Glue vs Step functions / Glue vs EMR [glue-VS-emr](https://www.trianz.com/insights/aws-glue-vs-emr)

Codepipeline/Codebuild

SNS Notification to CYBER Teams

CLoudwatch AWS Ressource Management / Cloudwatch/vs/Cloudtrail

QuickSight/vs/Grafana

Timestream/vs/InfluxDB/vs/DynamoDB/vs/TimescaleDB

Security and log analysis pipeline : AWS Systems Manager Parameter Store vs Timestram for storing logs ? 

Pour le la pipeline batch et cold storage/analysis ===> why not using lambda rather than EMR or athena 

and storing processed query to dynamo DB ===> answer : to moo datas for lambda but athena could be used on our redshift datawarehouse

IoT Analytics vs Kinesis Analytics

AWS Certificate Manager vs AWS Private CA

Aurora vs Redshift

Why not storing time series directly from Fleetwise to TimestreamDB ? Too high volume, too much vehicles, we want very high frequency raws datas, timestream will be too messy.

Load balancing, autoscaling the services (Firehorse, Streams etc …)

Security Like VPC, subnet, gateway for Services  