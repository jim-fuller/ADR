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
### - Use RDS Rread Replicas
### - Migrate to Aurora RDS Service

#### Decision
We will apply the RDS Multi AZ architecture to add high availability to our RDS production instances.



#### Consequences
This section describes the resulting context, after applying the decision. All consequences should be listed here, not just the "positive" ones. A particular decision may have positive, negative, and neutral consequences, but all of them affect the team and project in the future.

 
 
 

## Consequences
* One ADR describes one significant decision for a specific project. It should be something that has an effect on how the rest of the project will run.
* The consequences of one ADR are very likely to become the context for subsequent ADRs. This is also similar to Alexander's idea of a pattern language: the large-scale responses create spaces for the smaller scale to fit into.
* Developers and project stakeholders can see the ADRs, even as the team composition changes over time.
* The motivation behind previous decisions is visible for everyone, present and future. Nobody is left scratching their heads to understand, "What were they thinking?" and the time to change old decisions will be clear from changes in the project's context.