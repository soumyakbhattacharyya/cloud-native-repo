# Table of contents
1. [Scalability](#Scalability)
2. [Amazon Web Service](#AWS)
    1. [IAM](#IAM)
    2. [S3](#S3)
    3. [CDN](#CDN)
3. [System Design](#SystemDesign)
    * [Load Balancing](#LoadBalancing)
    * [Consistent Hashing](#ConsistentHashing) 
4. [Cloud Native Design](#CloudNative)

# Questions

## System Design 
### Scalability <a name="Scalability"></a>

* what is web hosting 
  * web hosting is a solution where your solution is served from a compute resource, shared with others
  * the difference between web hosting and infrastructure as service is, in case of web hosting it is impossible to choose the operating system of the compute, and also, there is no hypervisor involved which partitions a physical resource into smaller number of virtual machines 
* what is vertical scaling
  * vertical scaling is a way to beef up, available, RAM, CPU and Harddrive capacity of a machine 
* what is horizontal scaling
  * horizontal scaling refers to having a set of cheap commodity hardware that tackle the load by spreading it across them
  * uses multiple servers for the topology 
  * it requires a conduit to seat between internet and the set of servers; a black box called as load balancer, to distribute the inbound traffic to multiple servers
  * this way the load balancer requires to have public ip address
  * the server fleet can have private IP addresses
* what is load balancing & caching
  * load balancers typically distribute load to one of the multiple servers
  * load balancers can be hardware and software type
  * load balancers uses algorithm like round - robin to distribute traffic
  * if one of the worker process consumes a lot it is possible to have one server getting whole lot of requests to serve, eventually becoming to be a bottleneck
  * caching adds to this problem, by always redirecting requests from a specific host to one designated target
  * TTL or time to leave expires the cache affinity
* what is shared session state
  * now any shared session related solutions breaks as session is typically stored in first server where user's request landed, if the next time a different server is chosen, the session requires to be re-initialized 
* what is RAID
  * sessions can be stored in a shared file server using technology like RAID (Redundant Array of Indepedndent Disk)
  * RAID0
    * 2 identical harddrive where you stripe data accross them; which is effectively writing pieces of the whole data partly first into one drive and then to the next drive; the speed at which files are being written doubles
  * RAID1
    * 2 identical harddrive, where one drive works as mirror of the other
  * RAID5
    * variance of RAID1; versatile as consists of 3 - 5 drives where 1 is being used for redundancy 
  * RAID6
    * variance of RAID1; versatile as consists of 3 - 5 drives where 2 is being used for redundancy
  * RAID10
    * 4 identical drives, the solution is a combination of 0 and 1
* what is shared storage tech
  * NFS is a shared storage tech
* what is database replication
  * database replication can be used track metadata about user's session
* what is load balancing tech
  * load balancer options are ELB and HAProxy
  * hardwire load balancers are extremely expensive
* what is session affinity
  * cookies can offer solution to sticky session challenge (which is being able to always route traffic to the same server)
  * cookies are small in size; can we save id of the server inside the cookie 
  * cookies are stored in user's browsser and gets transmitted back to the server when request is being made
  * instead of putting IP address of the server (which serves the user's request), a random number can be inserted into the cookie by the load balancer itself so that when cookie comes back to the load balancer, it can find the random number, find the server that corresponds to the random number and route the request to appropriate destination server
* how to enable caching in mysql
  * by enabing query cache
* what is memcache
  * it is memory cache; runs in RAM; has support for all major programming platform
  * problem is cache can turn such big that it can not be further stored in RAM
* what are the storage engine of mysql
  * There are two chief storage engines of MySQL, MyISM and InnoDB; they are essentially two different storage mechanism
  * ÌnnoDB supports transaction and MyISM does not
  * The other options are memory - that stores the data in RAM; archive - it is compressed by default, slower than other engines; that way it saves space
* what is replication, what are the common topology around it
  * replication is creating copy of a master into slave record
  * the slaves can be load balanced while they primarily support read operations
  * load balancers in turn can stay in active - active or active - passive mode
  * active - active is when two load balancers are both actively serving traffic
  * active - passive is the scenario, when the passive one takes over the active one, after the active one dies
* what is partitioning 
  * partitionaing is a common paradim of splitting data into separate database schema 
* why switches are required
  * typically all machines (www, db, lb) connects via couple of switches (to ensure redundancy)
* why do we have redundant data center 
  * to avoid single point of failure problem for data centers
* what is cloning
  * Cloning is the first step towards scaling number of server instances that serve web application. Typically web servers deployed behind load balancers serves web requests. To accommodate scale number of web servers requires to be multiple in number. The number needs to scale in accordance to the increasing number of web requests coming in. This is the reason, why the applications should be stateless and keep user specific data like session state to external shared store like redis.
* what is the role of database behind scalability
  * once cloning has been achieved that next bottleneck will be database, if we are using mysql or any relational database, we need to scale database using master - slave model, where writes happen with master and reads happen with slaves. Beyond that sharding, denormalization and sql tuning are the way to achieve further scalability requirement. The alterative to this path is using NoSQL data stores, where the application has to make the joins in stead of database offering the same out of the box. But there is a cap to the scalability that may be achieved by this. Beyond this we will require caching.
* what is the role of caching as a factor that ensures scalability
  * Caching is the third armor to attain scalability. Caching usually happens in two of the available forms. Database caching is all about caching the query and resultset. It has the challenge of when to expire the cache, as there can be more than one factor that may require the cache to get invalidated. Object caching is the more favourable, whereby information fetched from database, are aggregated into an in - memory representation, and cached for a period of time to leave. Redis and memcahed are two solutions that supports this. Redis is favored over memcached for it's capability to behave as database.
* what is the role of asynchronism as a factor that ensures scalabality
  * Asynchronism is a way to well, do thing asynchronously.   
* what is difference between performance and scalability
  * If a system is slow in general, it is a performance problem, if it is fast for one user, but slow for multiple users, it is a scalability problem
* what is scalability
  * a service is said to be scalable if its capacity to process workload increases in proportion to the number of resources being added
  * for an always on service, where resources are being added to ensure redundancy, the service is said to be scalable, if adding resource does not reduce performance
* why is scalability so difficult to achive
  * scalability concerns requires to be taken into account right at the stage when software is being implemented
  * the axes that requires scalability needs to be identified upfront
  * measures taken to ensure redundancy and factors like growing heterogeneity of infrastructure platforms causes sclability to get affected; making it all the more harder
  
## System Design <a name="SystemDesign"></a>

* Foundational Concepts
  * System Design Basics
  * Key Attributes of Distributed System
  * Load Balancing <a name="LoadBalancing"></a>
    * http://soumyakbhattacharyya.github.io/2019/03/27/load-balancer-basic/
  * Caching
  * Data Partitioning 
  * Indexes
  * Proxies
  * Redundancy Vs. Replication
  * SQL Vs. NoSQL
  * CAP Theorem
  * Consistent Hashing <a name="ConsistentHashing"></a>
    * what is hashing
      * http://michaelnielsen.org/blog/consistent-hashing/
      * https://www.youtube.com/watch?v=viaNG1zyx1g
    * what is consistent hashing
      * consistent hashing is a way in which keys and nodes are mapped to same id space; through the use of similar hash function
      * the algorithm ensures that moving clockwise, a key is being mapped to nearest node
      * the key benefit of the mechanism is this way, if the hash function creates truely random outcome, keys are uniformrly distributed
      * another key benefit of consistent hashing over hash table is unlike hash table, the impact of addition or removal of a node is quite small. once, added or removed, the algorithm routes keys to newly added node if it is faster reachable than more distant node or keys are directed to next reachable node, should a node in between gets removed
      * this specific behaviour has potential to create cluster of keys on a specific node
      * the solution to this problem is creating virtual id for _A_ node, so that the node is found in multiple position and keys get routed to the nodes 
  * Long Polling Vs. Websocket Vs. Server Sent Event


* WhatsApp (Chat solution)
  * http://highscalability.com/blog/2014/3/31/how-whatsapp-grew-to-nearly-500-million-users-11000-cores-an.html
  * https://www.youtube.com/watch?v=vvhC64hQZMk
  * Key Concepts
    * API Gateway
    * Websocket
    * Load Balancer, Service Discovery, Authentication, Consistent Hashing, HTTP Websocket, Message Queue, Microservices, Polyglot Persistence

## AWS<a name="AWS"></a>
### IAM<a name="IAM"></a>

* what are the services offered by IAM
  * Centralized access management
  * Shared access to AWS account 
  * Granular access to AWS account
  * Federated access management via LinkedIn, Facebook etc.
  * MFA
  * Temporary access
  * Password rotation policy
  * Integrates with other AWS services
  * PSI DSS compliant
* what are users, groups, policies and roles, in brief
  * user is an entity who access a resource
  * group is collection of users; user inherits group permission
  * policy is a JSON representation of policy document; governs how user, group and roles access AWS resources
  * role is associated with one aws resource in terms of accessing another aws resource
* which is the region where latest products are lauched

  * US East (North Virginia)
* what is the sign in url; what is it being used for
  * the sign in url represents the link that can be shared with users to login to AWS account
  * this can be customized
  * this name is globaly unique
  * this is a publicly accessed url 
* what is the root account
  * god mode
  * should be guarded with MFA
* which region is guarded by IAM

  * global
* what are the details that require to be provided while creating new user
  * if the user is going to have programmatic (api driven) and / or aws management console acces 
  * what would be the password for the user 
  * if the user requires his password to be reset on first login
* how does administrative policy would look like
  * the administrative policy is looks as following
    * ```js
      {
       "Version": "1.0",
       "Statement": [
        {
          "Effect": "Allow",
          "Action": "*",
          "Resource": "*"
        }
       ]      
      }
      ```
* how can password generation and management be customized

  * by altering password policy
* what is roles

  * a way to ensure that one aws resource can access another aws service
* what are various policies
  * aws managed policy
  * user managed policy
  * job function based policies
* how can we restrict expenditure on aws
  * the expenses can be retricted by creating a billing alarm
  * before creating a billing alarm 'receive billing alert' needs to be enabled
  * navigate to cloudwatch
  * from billing section, select a matric EstimatedCharges and set a ceiling beyond which a notification is being sent


### Storage<a name="S3"></a>

* what is s3
  * S3 stands for Simple Storage Service
  * it is an object store capable of holding flat files
  * it is not suitable for hosting an operating system
  * file size can range from 0 kb - 5 Tb
  * files are stored in bucket
  * bucket names have to be globally unique
  * bucket access can be limited using bucket ACL (basic read/write access to other AWS account) or bucket policy (fine grained access to S3 resources)
  * succesful upload to S3 will return HTTP 200 code
  * the consistency models are Read after Write consistency for PUT with new object & Eventual consistency for overwrite PUT and DELETE
  * delete operations can be guarded using MFA Delete
  * Content of file
    * Key
    * Value
    * Version Id
    * Metadata
    * Subresources
  * There are 6 storage classes
    * S3 standard - 99.99% available, 11-9 durable, distributed and design to sustain concurrent failure of 2 data centers
    * S3 IA - for data that is accessed less frequently, but needs to be accessed fast
    * S3 One Zone IA - S3 IA with one zone
    * S3 Intelligent Tiering - uses machine learning to shift data across various storage class
    * S3 Glacier - secure, durable, low - cost storage class for data archival 
    * S3 Glacier Deep Archive - takes 12 hours to get data being retrieved 
  * Explain basics of encryption for S3
    * Encryption are of two kinds: Encryption in transit & Encryption at rest (Server Side)
    * Encryption in transit is achived using SSL/TLS
    * Encryption at rest is achived by 
      * 3 flavours of Server Side Encryption (SSE)
         * Where AWS manage the encryption key - SSE-S3
         * Where customer and AWS jointly manage the key using Amazon Key Management Service - SSE - KMS
         * Where customer manages the key all by itself - SSE - C
      * Client managed encryption
  * Explain versioning in S3
    * Versioning is a property that requires to be enabled on a S3 bucket
    * Versioning once enabled can only be suspended and not removed
    * Versioning helps to keep all versions of an object
    * However, public access requires to be enabled on individual version of an object; as it is not inherited from parent
    * Deleting an object, puts a delete marker, which in turn is a new version after latest
    * Delete marker is a soft delete mechanism, and to restore back a file, delete marker needs to be deleted
    * Deleting a version specifically removes the version permanently from S3
  * Explain lifecycle rules in S3
    * Lifecycle rules governs the lifecycle management of object, in a bucket
    * Having 'Versioning' turned on, is not a pre-requisite for the configuring lifecycle rule, however, having it, would make current & previous versions open to being lifecycle rule managed
    * It manages transition of object from one class of storage to the other
    * it manages expiration of object
    * can have tag, objects with the same tag, gets subjected to the lifecycle rule
    * this can be configured for current and previous version
      * multiple transition can be configured to move the object from one class of storage to the other, after configured number of days have elapsed, counted since the time the object was created
    * the expiration of the object can be configured as well (for current and previous versions)

### Content Delivery Network<a name="CDN"></a>
* what is edge location
  * edge location are those servers which caches the content; different than region/availabiity zone
* what is orgin
  * denotes the source from where the definitive version of the content is being served; can be S3/EC2,Route53 etc.
  * in case of web content, S3/HTTP server running on EC2 can serve as origin
  * for content that are being delivered over Amazon Media Server RTMP protocol, the origin is always S3
* what is distribution
  * the CDN which furnishes and distributes the file from origin to edge location
* what is cloudfront
  * cloudfront is a content distribution network, that can distribute content served from S3, EC2, Route53 and aws external origin server
* how many types of distribution mechanism are there 
  * web & rtmp (to stream media files using Adobe Flash Media Server's RTMP protocol)
* what are the common use cases for using cloudfront
  * to serve web application content
  * to serve on demand videos or live streams
  * to encrypt specific fields in the TBD
  * to customize at the endge using lambda running at the edge location
* how does cloudfront delivers content
  * there are couple of ways in which cloudfront deliveres contents
    * when a user request arrives, the request is precessed via aws backbone. edge location which can serve the content with minimum latency, is chosen to serve the request. the edge location verifies if the file is in it's cache, if yes starts serving it, if not, it forwards the request to origin server to serve the content
    * to ensure faster serving of popular content, a regional edge cache is designed that sits in between the origin server and regional edge location; typically the regional edge cache has higher capacity to cache than regional edge location; making sure that most popular contents are being furnished right from the regional edge location
    * the mechanism of getting content is similar; note, all proxy HTTP methods are directed straight to the origin via the edge location and does not go via regional edge cache
* how are user billed for using cloudfront
  * user is billed for storing data on S3, GETting content from cloudfront and performing PUT, POST, DELETE, PATCH, OPTION - ing to cloudfront
  * it is possible to restrict user to be able to interact with cloudfront edge location which are cheaper than the other; at the cost of higher expense; however AWS ensures that user gets the content from the edge location with minimum latency, from the chosen cloudfront region

### EC2<a name="EC2"></a>

- what are prevalent pricing model for EC2
  - on demand - pay a fixed rate per hour (or seconds) with no commitment
  - reserve - have a 1 - 3 years contract with AWS for a capacity reservation, significant reduction in cost
  - spot - bid for a price you want to capacity, and you get when the capacity is available in excess, also instances provisioned for you is revoked back, when demand surges; application with flexible start and end time can have these sort of instance, can be availed really cheap. remember in case of spot instances, if AWS terminates it, you do not have to pay for the hour, but if you terminate it, you need to pay for full hour during which you used it
  - dedicated host - you get a physical server dedicated to you, useful when one needs to continue using his server bound licenses
- what are reserved pricing types
  - standard
    - in this case, instance type of the reserved instances can not be altered in future; offers 75% savings over on demand
  - convertible
    - in this case, instance type can be changed; offers 54% savings
  - scheduled
    - available to be running during a schedule period of time
- what are the instance type and what are the reason, they are being used for
  - FIGHTDRMCPXZAU (fight dr. mac pixz au)
    - F for FPGA
    - I for IOPS
    - G for Graphics
    - H for High Disk Throughput
    - T for General purpose
    - D for Density 
    - R for RAM
    - M for main choice for general purpose app
    - C for Compute
    - P for Graphics (Pix)
    - X for Extreme memory
    - Z for Extreme memory and CPU
    - A for ARM based processor
    - U for Bare Metal
- what are the key facts around EC2 instance
  - termination protection is turned on (you can not terminate, by default)
  - root volumes are deleted once the machine is terminated
  - root volume can not be encrypted 
  - non - root volumes can be encrypted
- what is security group
  - security group is a mechanism to define a local DMZ for the instance. 
  - all inbound traffics are blocked by default, one needs to enable it, and it takes immediate effect
  - allowing an inbound traffic, auto - allow outbound traffic too; deleting outbound traffic has no effect
  - instance and security group has M-N relationship
  - security group can not blacklist individual IP address or port
  - security group can be attached with a running machine 
- what is EBS (elastic block storage)
  - elastic block storage is the hard drive for EC2 instances
  - comes in 2 flavours
    - SSD
      - general purpose - takes up most workload, api name is gp1, volume is 1 GiB - 16 TiB; supports 16000 IOPS
      - Provisioned IOPS SSD - backend for mission critical app, io1 is the api name, very high IOPS
    - HDD
      - Throughput optimized - for Big Data
      - Cold - for file server
      - Magnetic - legacy
  - Volumes associated with an instance exists in EBS, EBS is virtual hard drive on cloud
  - Snapshot of volume can be taken, the snapshot thus created exists in S3
  - Snapshot of volume is incremental 
  - While creating a snapshot for a volume that serves as root device for th instance, the instance is recommended to be stopped
  - EBS volumes get created in the same availability zone, as that of the machine
  - To migrate volumes, option 1 is to create snapshot of the volume, create AMI from it, and instantiate a new VM from the AMI to a new availability zone, option 2 is to create a snapshot of the volume, create AMI from it, and copy the AMI to new availability zone, and instantiate a machine in new zone
  - Note, AMI can be created from volume or snapshot
  - The size and type of volume can be changed at runtime
- what is instance store
  - it is possible to boot instances from AMI that supports instance store (instead of EBS store backed)
  - instance stores are called ephemeral store, because if underlying hypervisor crashes, the data on instance store gets lost
  - instance store backed instances can be rebooted and not stopped
  - root volume irrespective of whether it sits on EBS store or instance store will get terminated
- explain encryption in relation to volume
  - snapshot of encrypted volume is encrypted automatically and volume restored from encrypted snapshot are encrypted
  - snapshot only if unencrypted can be shared with other account
  - to create an encrypted volume from an unencrypted volume, first snapshot of the unencrypted volume requires to be taken, then the snapshot need to be copied and while doing a copy encrypt option must be selected; after that if we create an AMI from the snapshot the AMI will always have encrypted volume

## Cloud Native Solution Architecture <a name="CloudNative"></a>  
### References

* https://medium.com/@FedakV/cloud-native-vs-lift-and-shift-which-way-to-choose-7dc89e1da906
* https://medium.com/aws-enterprise-collection/cloud-native-or-lift-and-shift-99970053b25b
* https://medium.com/aws-enterprise-collection/4-reasons-to-re-consider-lift-and-shift-to-the-cloud-e7cde603941e
* https://medium.com/blue-harvest-tech-blog/a-swift-lift-and-shift-56bac50a04dd
* https://blog.tidalmigrations.com/the-challenge-with-lift-and-shift-cloud-migrations-a6a9ca76d4c1
* https://medium.com/devopslinks/the-fallacy-of-lift-and-shift-in-data-centre-migrations-fd73e2010736
* https://medium.com/slalom-technology/is-lift-shift-actually-a-quick-and-painless-path-to-the-cloud-84084548f0d7
* https://medium.com/aws-enterprise-collection/6-strategies-for-migrating-applications-to-the-cloud-eb4e85c412b4

### Why Cloud Native solution architecture is required




### What is Cloud Native solution architecture
### How to achive Cloud Native solution architecture
### What are the foundational design patterns in Cloud Native solution architecture
### What are the boundary design patterns in Cloud Native solution architecture 
### What are the control design patterns in Cloud Native solution architecture 
### What are the deployment pattern in Cloud Native solution architecture 
### What are the testing pattern in Cloud Native solution architecture 
### What are the monitoring pattern in Cloud Native solution architecture 
### What are the security related patterns in Cloud Native solution architecture

