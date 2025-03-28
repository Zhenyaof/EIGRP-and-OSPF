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


```markdown
# Implement Advanced EIGRP for IPv4 Features  

## Overview  
This lab focuses on **advanced EIGRP for IPv4** features, including customizing timers, route summarization, query propagation control, and route filtering. By modifying these parameters, you will **optimize EIGRP performance, enhance scalability, and reduce unnecessary traffic** in large-scale networks.  

## Objectives  

### **Part 2: Implement EIGRP for IPv4**  
- Enable and verify **EIGRP for IPv4** operation.  
- Establish **neighbor adjacencies** and check routing table updates.  

### **Part 3: Implement Advanced EIGRP Features**  
- **Modify EIGRP timers** to adjust convergence speed.  
- **Create summarized routes** to optimize routing tables.  
- **Control EIGRP query propagation** using stub routers.  
- **Filter EIGRP routes** with a distribute list.  


### **Initial Setup**  
Ensure the following configurations before starting:  
- **IP addressing** is configured.  
- **EIGRP AS 100** is enabled across all routers.  
- **Full connectivity** is established using EIGRP.  

---

## **Configuration Steps**  

### **Step 1: Modify EIGRP Timers**  
Adjust **hello and hold timers** to control how quickly EIGRP detects failures.  
```bash
interface GigabitEthernet0/0
 ip hello-interval eigrp 100 5
 ip hold-time eigrp 100 15
```
- Reducing the **hello interval** makes failure detection faster.  
- The **hold-time** should be at least three times the hello interval.  

### **Step 2: Create Summarized Routes in EIGRP**  
Manually summarize networks to reduce routing table size and enhance efficiency.  
```bash
interface GigabitEthernet0/0
 ip summary-address eigrp 100 192.168.0.0 255.255.252.0
```
- **Summarization reduces EIGRP updates**, improving performance.  
- This creates a single **/22 route** instead of multiple smaller subnets.  

### **Step 3: Control Query Propagation with EIGRP Stub Routers**  
Stub routers help **limit unnecessary queries** in large networks, reducing overhead.  
```bash
router eigrp 100
 eigrp stub connected summary
```
- The **stub router only advertises directly connected and summarized routes**.  
- This prevents it from being queried for unreachable destinations.  

### **Step 4: Filter EIGRP Routes with a Distribute List**  
Use **distribute lists** to control which routes are advertised.  
```bash
access-list 10 deny 192.168.3.0
access-list 10 permit any
router eigrp 100
 distribute-list 10 out GigabitEthernet0/1
```
- Blocks **192.168.3.0/24** from being advertised out **GigabitEthernet0/1**.  
- Prevents unnecessary route propagation in specific parts of the network.  

---

## **Verification Commands**  
Use the following commands to **validate and troubleshoot** your configuration:  
```bash
show ip eigrp interfaces
show ip eigrp neighbors
show ip eigrp topology
show ip eigrp traffic
show ip protocols
```
- Check **EIGRP neighbor relationships** and interface participation.  
- Verify **route summarization** and **filtered advertisements**.  
- Ensure stub routers **do not receive unnecessary queries**.  

---

## **Summary**  
By completing this lab, you have:  
 Adjusted **EIGRP timers** for faster convergence.  
Implemented **manual summarization** to optimize routing tables.  
Controlled **query propagation** using EIGRP stub routers.  
Filtered **unwanted route advertisements** with distribute lists.


```markdown
# Implement Multi-Area OSPFv2  

## Overview  
This lab focuses on **configuring and verifying Multi-Area OSPFv2**, understanding **link-state advertisements (LSAs)**, and exploring how OSPF routers maintain the **link-state database (LSDB)**. You will configure **multi-area OSPF** across multiple routers and analyze how different LSA types contribute to the overall routing topology.  

## Objectives  

### **Part 2: Configure and Verify Multi-Area OSPF for IPv4**  
- Implement **OSPFv2 multi-area** on routers **R1, D1, and D2**.  
- Establish OSPF neighbor adjacencies across multiple areas.  
- Verify **OSPF route propagation** and **area segmentation**.  

### **Part 3: Exploring Link-State Advertisements (LSAs)**  
- Examine how **OSPF LSAs** build the **link-state database (LSDB)**.  
- Verify the role of different **LSA types** in OSPF topology formation.  
- Analyze **OSPF route summarization** and area-based routing decisions.  

---

## **Configuration Steps**  

### **Step 1: Enable Multi-Area OSPF on R1, D1, and D2**  
```bash
router ospf 1
 router-id 1.1.1.1
 network 10.0.0.0 0.0.0.255 area 0
 network 192.168.1.0 0.0.0.255 area 1
```
- **Area 0 (backbone area)** ensures proper inter-area communication.  
- **Area 1 (non-backbone area)** is used for segmentation.  

### **Step 2: Verify OSPF Neighbor Adjacencies**  
Check if routers in the same area have formed **OSPF neighbor relationships**.  
```bash
show ip ospf neighbor
```
- Adjacencies should be **FULL** for a stable OSPF topology.  

### **Step 3: Exploring Link-State Advertisements (LSAs)**  
OSPF uses **LSAs** to propagate routing information and maintain the LSDB.  

#### **LSA Types and Their Functions**  

1. **Type 1 – Router LSA:**  
   - Generated by **all OSPF routers** for their own interfaces.  
   - Remains **within the same area** and does not cross ABRs.  

2. **Type 2 – Network LSA:**  
   - Generated by the **Designated Router (DR)** in multi-access networks.  
   - Lists all connected routers on the same subnet.  

3. **Type 3 – Summary LSA:**  
   - **ABRs** generate this to summarize **intra-area routes** into other areas.  
   - Helps reduce **LSDB size** and improves network efficiency.  

#### **Viewing LSAs in the Database**  
```bash
show ip ospf database
```
- Displays the **LSDB** containing all received LSAs.  
- Helps verify **network convergence**.  

---

## **Verification Commands**  
Use the following commands to analyze and troubleshoot the OSPF network:  

```bash
show ip route ospf
show ip ospf interface
show ip ospf neighbor
show ip ospf database
```
- **Check OSPF routes** in the routing table.  
- **Confirm adjacency states** between routers.  
- **Analyze LSAs** and their role in the network.  

---

## **Summary**  
By completing this lab, you have:  
 Configured **Multi-Area OSPFv2** with multiple OSPF areas.  
 Established **OSPF neighbor adjacencies** across routers.  
 Explored **LSA types 1, 2, and 3** to understand how OSPF builds the **LSDB**.  
 Verified **OSPF route propagation and summarization**.  


