# Questions

## System Design
### Scalability

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

  

## AWS
### IAM

* what are the services offered by IAM
* what are users, groups, policies and roles, in brief
* what is the sign in url; what is it being used for
* what is the root account
* which region is guarded by IAM
* what are the two kinds of permission that the newly created user has choise to be associated with
* how can we assert the restriction on the password policies
* what is user name, password, access key and secret access key
* what are roles
* why roles are being used for
* how can you create a new billing alarm using cloudwatch

#### Notes

* IAM consists of User, Groups, Roles and Policies
* Roles are for one AWS service to call another service
* Permission is granted through policies
* Policies are JSON document 
* IAM is universal; agnostic of any specific region
* root account is the one when you first set up AWS account
* new users have no permission by default
* new users get access key and secret key access key to be able to use the API
* always set up multi - factor authentication on root account
* it is possible to create and customize own password rotation policy

### Storage

* what is s3
  * Simple Storage Service is a secure, durable and highly - scalable object storage
* can you install operating system etc. in s3
  * no, S3 is an object store and not a block based store
* what is the highest size of file stored in S3
  * 5 tb
* what is a bucket
  * a bucket is a folder structure within S3
* what is difference between bucket and folder
  * bucket has a global unique name of the following format http://s3-region.amazonaws.com/bucket-name
* which http status code is returned when file is updated
  * 200
* what is the consistency model
  * for new object read happens after write (read after write consistency)
  * eventual consistency for overwrite puts and deletes 
* what are various attributes of object
  * key (the name)
  * value
  * version id
  * metadata
  * subresources
    * acl
    * torrent
* what is the SLA for S3
  * 99.99% for availability
  * 11-9 durability for files
  * tiered storage
  * lifecycle management
  * versioning
  * encryption
  * controllable using acl (for individual file) or bucket control list 
* what are storage tiers
  * S3 Standard (has no retrival cost)
  * S3 IA (infrequently accessed)
  * S3 One Zone - IA - kept in one zone, cheaper than former two
  * glacier - Expedited, Standard or Bulk; issue is the retrieval time 
* what are we charged for S3
  * storage
  * requests
  * storage management pricing 
  * data transfer pricing 
* what is S3 transfer accelaration
  * S3 transfer accelaration is a method using which user uploads file to closest edge location; further the files are trasferred to target S3 bucket using Amazon Network backbone. This way users do not have to directly upload files to target S3 bucket.
* can we deploy the s3 to specific region
  * yes
* after creating a bucket with default setting for public access, how can we make an object belonging to the bucket public?
  * by default a bucket and object are private
  * an object can not be made publicly accessible unless, the bucket that contains it is publicly accessible
* what is versioning
  * versioning is a way of keeping multiple version of same object inside an S3 bucket
* after enabling versioning can you disable it
  * no, after being enabled versioning can only be suspended
* what is lifecycle policy for the bucket
  * the lifecycle policy of an bucket, defines rules that applies to object, in terms of when the objects get transitioned to a cheaper storage class
  * can be done, with or without versioning
  * can be applied with current and previous version
  * can be set to expire 
* what is server access logging
* what is object level logging
* what is default encryption
* what is object lock
* what is tags
* what is events
* what is requester pays
* what is cross region replication
  * cross region replication is a way to save selected or entire content of a bucket into another bucket in different region
  * note that cross region replication, will affect new & updated files only, delete does not get automatically replicated
  * delete markers are not replicated
  * deleting individual file or version will not be replicated 
  
