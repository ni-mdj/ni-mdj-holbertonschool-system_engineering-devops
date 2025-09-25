# Web Infrastructure Design

This project contains whiteboard designs and explanations for different web infrastructure setups.  
Each task builds on the previous one, making the architecture more scalable, secure, and reliable.  

All explanations are written in English to improve technical communication skills.  

---

## **0. Simple Web Stack**

A simple web infrastructure with a single server.  

### Requirements  
- 1 domain name **foobar.com** configured with a **www record** pointing to the server IP `8.8.8.8`  
- 1 server containing:  
  - 1 web server (**Nginx**)  
  - 1 application server  
  - 1 set of application files (the codebase)  
  - 1 database (**MySQL**)  

### Explanations  
- **Server**: A physical or virtual machine that hosts applications and services.  
- **Domain name**: A human-readable address (foobar.com) that maps to the server’s IP.  
- **DNS record**: The `www` in `www.foobar.com` is an **A record**, pointing to `8.8.8.8`.  
- **Web server (Nginx)**: Handles HTTP requests, serves static files, and passes dynamic requests to the application server.  
- **Application server**: Executes the application logic (PHP, Python, Ruby…).  
- **Application files**: The actual website source code.  
- **Database (MySQL)**: Stores and manages persistent data.  
- **Communication**: Between the user and the server is done through the **HTTP protocol**.  

### Issues  
- **Single Point of Failure (SPOF)**: If the server goes down, the website is unavailable.  
- **Maintenance downtime**: Restarting or updating the server causes downtime.  
- **Scalability issues**: Cannot handle high traffic.  

---

## **1. Distributed Web Infrastructure**

A more resilient setup with a load balancer and multiple servers.  

### Requirements  
- 1 load balancer (**HAProxy**)  
- 2 servers, each containing:  
  - 1 web server (**Nginx**)  
  - 1 application server  
  - 1 set of application files  
  - 1 database (**MySQL**)  

### Explanations  
- **Load balancer (HAProxy)**: Distributes traffic across servers to avoid overload.  
- **Distribution algorithm**: Example → **Round Robin**, which cycles requests between servers.  
- **Active-Active setup**: Both servers handle traffic simultaneously.  
- **Active-Passive setup**: One server is active, the other stays on standby.  
- **Database cluster (Primary-Replica)**:  
  - **Primary (Master)**: Handles writes.  
  - **Replica (Slave)**: Copies data from the Master, handles reads.  

### Issues  
- **SPOF**: The load balancer itself is a single point of failure.  
- **No security**: No firewalls, no HTTPS.  
- **No monitoring**: Failures are not automatically detected.  

---

## **2. Secured and Monitored Web Infrastructure**

A secure and monitored infrastructure.  

### Requirements  
- 3 firewalls (one for each server)  
- 1 SSL certificate to serve **www.foobar.com** over **HTTPS**  
- 3 monitoring clients (to collect metrics)  

### Explanations  
- **Firewalls**: Protect servers by filtering unauthorized traffic.  
- **HTTPS (SSL certificate)**: Encrypts communication between the user and the website.  
- **Monitoring**: Detects failures, performance bottlenecks, and traffic levels.  
- **Monitoring data**: Metrics such as CPU usage, memory, error rate, and QPS (Queries Per Second) are collected by agents and sent to a central system.  
- **Monitoring QPS**: Configure web server monitoring agents to measure the number of requests processed per second.  

### Issues  
- **SSL termination at load balancer**: Traffic between load balancer and servers is unencrypted.  
- **Database write bottleneck**: Only the Master accepts writes.  
- **Same components on each server**: Running web server, app server, and database together increases complexity.  

---

## **3. Scale Up**

A fully scaled infrastructure with separated components.  

### Requirements  
- 1 additional server  
- 1 new load balancer (**HAProxy**) configured in a **cluster** with the first one  
- Split components so that each service runs on its own server:  
  - Web servers (Nginx only)  
  - Application servers  
  - Database servers  

### Explanations  
- **Load balancer cluster**: Two load balancers avoid SPOF, working in Active-Active mode.  
- **Separation of concerns**: Each type of server has a single role (web, app, or database).  
- **Independent scaling**:  
  - Add more web servers for high traffic.  
  - Add more application servers for heavy logic.  
  - Add more database replicas for read-heavy workloads.  
- **Resilience**: The system continues working even if one load balancer or one server fails.  

---

