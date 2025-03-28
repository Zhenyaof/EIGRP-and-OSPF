# EIGRP and OSPF

# About This Repository  
Welcome to the **EIGRP and OSPF** repository—an in-depth resource designed for networking professionals and engineers aiming to master EIGRP and OSPF routing protocols. This repository covers advanced configurations, real-world troubleshooting scenarios, and best practices to enhance routing efficiency and network stability.  

## Key Focus Areas  

### **EIGRP (Enhanced Interior Gateway Routing Protocol)**  
EIGRP is an advanced distance-vector routing protocol that optimizes network performance using a sophisticated metric calculation. This repository explores:  
- **Basic and Advanced Configurations**: Setting up EIGRP autonomous systems, verifying neighbor adjacencies, and optimizing metric calculations.  
- **EIGRP Stub Routing**: Configuring stub networks to minimize unnecessary route advertisements and improve efficiency.  
- **EIGRP Authentication**: Implementing MD5 authentication to secure EIGRP neighbor relationships and prevent spoofing.  
- **Route Optimization & Summarization**: Implementing manual summarization, filtering, and unequal-cost load balancing.  

### **OSPF (Open Shortest Path First)**  
OSPF is a link-state routing protocol designed for scalability and fast convergence in large networks. Topics covered include:  
- **Single-Area and Multi-Area OSPF**: Configuring and troubleshooting networks with different area structures.  
- **OSPF Authentication**: Implementing MD5 and plain-text authentication to enhance security.  
- **Route Summarization & Filtering**: Reducing routing table size through manual summarization at area boundaries.  
- **OSPF Virtual Links**: Extending Area 0 connectivity using virtual links for seamless network expansion.  
- **NSSA (Not-So-Stubby Area)**: Configuring NSSA to allow controlled external route injection.  

### **Route Redistribution and Optimization**  
- **Redistributing Routes Between Protocols**: Configuring EIGRP-OSPF redistribution with route filtering to avoid routing loops.  
- **Performance Tuning**: Adjusting timers, bandwidth, and delay settings for better convergence.  

### **Security Best Practices for Routing Protocols**  
- Implementing authentication to protect routing information from unauthorized alterations.  
- Hardening router configurations to prevent common attack vectors such as route poisoning.  
- Using prefix-lists and access control lists (ACLs) to secure route advertisements.  

## Lab Environment  
Labs are designed for compatibility with **Cisco Packet Tracer, GNS3, and EVE-NG**, complete with detailed instructions for setup, execution, and troubleshooting. Each lab includes real-world scenarios to reinforce networking concepts and enhance troubleshooting skills.  

## Important Notice  
This repository is intended for educational and training purposes only. Any IP addresses, usernames, or passwords used in configurations are for simulation purposes and should not be used in production environments. Ensure all network security practices align with ethical guidelines and compliance requirements.



```markdown
# Implement EIGRP for IPv4 

## Overview  
This lab focuses on configuring, verifying, and optimizing **EIGRP for IPv4** in a simulated enterprise network. You will implement best practices for **secure and efficient EIGRP routing**, ensuring optimized path selection and protection against unauthorized route updates.  

## Objectives  

### **Part 2: Configure and Verify EIGRP for IPv4**  
- Enable and configure **EIGRP for IPv4** on network routers.  
- Establish **neighbor relationships** and verify adjacency formation.  
- Implement **manual summarization** to optimize routing table efficiency.  
- Use **show commands** to validate and troubleshoot EIGRP behavior.  

### **Part 3: Tune and Optimize EIGRP for IPv4**  
- **Passive Interfaces**: Secure EIGRP by disabling updates on unnecessary interfaces.  
- **Authentication**: Configure MD5 authentication to prevent unauthorized route updates.  
- **Variance**: Implement load balancing over unequal-cost paths for optimal resource utilization.  

## Lab Topology  

This lab assumes a topology with at least three routers connected via multiple paths. Ensure that each router has **IP connectivity**, and EIGRP is enabled across all participating interfaces.  

```
[R1] -------- [R2] -------- [R3]
  |                          |
[R4] ---------------------- [R5]
```

### **Initial Setup**  
Ensure the following before proceeding:  
- Basic **IP addressing** is configured.  
- Routing **EIGRP AS 100** is enabled.  
- All routers have **full connectivity** via EIGRP before tuning begins.  

---

## **Configuration Steps**  

### **Step 1: Enable and Configure EIGRP**  
```bash
router eigrp 100
 network 192.168.1.0 0.0.0.255
 network 192.168.2.0 0.0.0.255
 no auto-summary
```
- The **network statements** enable EIGRP on specific subnets.  
- `no auto-summary` ensures classless routing and prevents automatic summarization.  

### **Step 2: Verify EIGRP Neighbor Adjacency**  
```bash
show ip eigrp neighbors
show ip eigrp topology
show ip route eigrp
```
- **Check neighbor table** to confirm adjacency establishment.  
- **Examine topology table** for feasible successors.  
- **Review routing table** for EIGRP-learned routes.  

---

## **Part 3: Optimizing EIGRP**  

### **Step 3: Secure EIGRP with Passive Interfaces**  
Disable EIGRP updates on interfaces that do not require routing advertisements.  
```bash
router eigrp 100
 passive-interface GigabitEthernet0/1
```
- Prevents **unnecessary hello packets** and secures routing updates.  

### **Step 4: Implement MD5 Authentication**  
Enhance security by configuring authentication between EIGRP neighbors.  
```bash
key chain EIGRP-Auth
 key 1
  key-string mysecurekey
!
interface GigabitEthernet0/0
 ip authentication mode eigrp 100 md5
 ip authentication key-chain eigrp 100 EIGRP-Auth
```
- **Key chains** define secure authentication credentials.  
- **MD5 authentication** prevents unauthorized routing updates.  

### **Step 5: Configure EIGRP Variance for Unequal-Cost Load Balancing**  
Allow traffic to take multiple paths based on metric calculations.  
```bash
router eigrp 100
 variance 2
```
- Allows load balancing over **paths with metrics up to twice the lowest cost**.  
- Helps distribute **traffic efficiently across network links**.  

---

## **Verification Commands**  
Use these commands to validate and troubleshoot your EIGRP optimization:  
```bash
show ip eigrp interfaces
show ip eigrp topology
show ip eigrp traffic
show run | section eigrp
```
- **Verify interface participation** in EIGRP.  
- **Check routing behavior** after tuning changes.  
- **Analyze traffic statistics** to confirm load balancing and security settings.  

---

## **Summary**  
By completing this lab, you have:  
 Configured and verified **EIGRP for IPv4**.  
 Secured routing updates using **passive interfaces and MD5 authentication**.  
Optimized path selection with **variance for unequal-cost load balancing**.  

