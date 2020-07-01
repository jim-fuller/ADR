# ADR - 0001 Choosing an AWS RDS High Availability Solution

Date: 2020-07-01

## Status

Proposed

## Context
Our Production AWS RDS databases are not configured for high availablity. There exists a few ways to provide HA on RDS instances. We cover those in this document with the goal of choosing, among other criteria, an offering that aligns with AWS archtectural best practices. 

### Distinguishing between HA and DR:
High Availability (HA) provides a failover solution in the event a database, vpc, or availability zone fails. Disaster Recovery (DR) provides a recovery solution across a geographically separated distance (multi-region) in the event of a disaster that causes an entire data center to fail.

In this ADR, we select an architecture that ensures High Availability and defer Disaster Recovery to a separate ADR.

## Criteria
Choosing the right HA architecture should 1) align with AWS Best Architectural Practices, 2) is cost effective, and 3) minimizes or eliminates disruption to Redline Production applications using RDS.

## Proposed Solutions

### - Use the RDS Multi AZ
Pros:  
- Syncronous Replication for instant db consistency at failover (hot standby) and minimal db downtime
- One DNS Name across all standby replicas eliminates application intervention
- 3 levels of failover: 1)vpc network, 2) db or ebs crashes, or 3) AZ unavailibility.

Cons:  
- Zero HA when AWS Region fails
- Likely Doubles cost of RDS for each hot replica, and the network traffic to sync data.
- Perhaps some write latency during synchonous writes to standby replicas
- Not a scaling solution

### - Use RDS Read Replicas
It is possible to create a failover solution with read replicas, though this is typically done when read replicas have already been deployed into the environment  

Pros:  
- Move Anayltic or Reporting workloads off of master db on to RR's
- Replicas can be promoted to their own db's
- Asyncronous reads create delayed consistencies on replicas but provide zero latency writes  

Cons:
- Apps must update connection config
### - Migrate to Aurora RDS Service
Pros:  
Cons:

#### Decision
We will apply the RDS Multi AZ architecture to add high availability to our RDS production instances as it is the recommended best practice to adding HA to RDS.



#### Consequences
This section describes the resulting context, after applying the decision. All consequences should be listed here, not just the "positive" ones. A particular decision may have positive, negative, and neutral consequences, but all of them affect the team and project in the future.

 
 
 

## Consequences
* One ADR describes one significant decision for a specific project. It should be something that has an effect on how the rest of the project will run.
* The consequences of one ADR are very likely to become the context for subsequent ADRs. This is also similar to Alexander's idea of a pattern language: the large-scale responses create spaces for the smaller scale to fit into.
* Developers and project stakeholders can see the ADRs, even as the team composition changes over time.
* The motivation behind previous decisions is visible for everyone, present and future. Nobody is left scratching their heads to understand, "What were they thinking?" and the time to change old decisions will be clear from changes in the project's context.