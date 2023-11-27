# OpenMetrics Security Self Assessment 
The Self-assessment is the initial document for projects to begin thinking about the
security of the project, determining gaps in their security, and preparing any security
documentation for their users. This document is ideal for projects currently in the
CNCF **sandbox** as well as projects that are looking to receive a joint assessment and
currently in CNCF **incubation**.

For a detailed guide with step-by-step discussion and examples, check out the free 
Express Learning course provided by Linux Foundation Training & Certification: 
[Security Assessments for Open Source Projects](https://training.linuxfoundation.org/express-learning/security-self-assessments-for-open-source-projects-lfel1005/).

# Self-assessment outline

## Table of contents

* [Metadata](#metadata)
  * [Security links](#security-links)
* [Overview](#overview)
  * [Actors](#actors)
  * [Actions](#actions)
  * [Background](#background)
  * [Goals](#goals)
  * [Non-goals](#non-goals)
* [Self-assessment use](#self-assessment-use)
* [Security functions and features](#security-functions-and-features)
* [Project compliance](#project-compliance)
* [Secure development practices](#secure-development-practices)
* [Security issue resolution](#security-issue-resolution)
* [Appendix](#appendix)

## Metadata

|   |  |
| -- | -- |
| Software | https://github.com/OpenObservability/OpenMetrics |
| Security Provider | No |
| Languages | Go-89.1%, HTML-4.9%, Makefile-3.6%, Sass-1.7%, Python-0.3%, Dockerfile-0.3%, Ruby-0.1%  |
| SBOM | https://github.com/OpenObservability/OpenMetrics/blob/main/src/go.mod |
| | |

### Security links

| Doc | url |
| -- | -- |
| Security file | https://github.com/OpenObservability/OpenMetrics/blob/main/specification/OpenMetrics.md |
| Default and optional configs | https://github.com/OpenObservability/OpenMetrics/blob/main/.editorconfig |

## Overview

OpenMetrics is a specification built upon and carefully extending Prometheus exposition format in almost 100% backwards compatible ways. 

### Background

Prometheus is an open source software that provides monitoring and alerting functionality for cloud-native environments, including Kubernetes. It can collect and store metrics as time-series data, recording information with a timestamp. Metrics can be exposed to Prometheus using a simple text-based exposition format. Metrics are a specific kind of telemetry data. They represent a snapshot of the current state for a set of data. They are distinct from logs or events, which focus on records or information about individual events. OpenMetrics is primarily a wire format, independent of any particular transport for that format. The format is expected to be consumed on a regular basis and to be meaningful over successive expositions. Implementers MUST expose metrics in the OpenMetrics text format in response to a simple HTTP GET request to a documented URL for a given process or device. This endpoint SHOULD be called "/metrics". Implementers MAY also expose OpenMetrics formatted metrics in other ways, such as by regularly pushing metric sets to an operator-configured endpoint over HTTP.

### Actors
 In this Open Matrics, four different actors are introduced. To structure the information better, several metrics type is also introduced to help the understanding.
	First, the Datatype is introduced and should be ensured when implementing under the open matrics environment. For example, the values, boolean, and many other data types that you can see in other languages should follow a certain included method, not based on your local requirements. Failing to follow this Data type requirement will result in having an exception that you are using an invalid datatype, which will ruin the compile process in transmitting.
	Second, a text format is introduced as the second actor in order to maintain the running capacity and text normalizing. With the requirement, UTF-8 MUST be used. Byte order markers (BOMs) MUST NOT be used. Each structure, for example, the Escaping and Numbers, should also follow the Open Matrics requirement in order to ensure text normalization. There is also a text format requirement in using Metrics types and this should be ensured as well.
	Third, the Protobuf format is also required as the third actors of this project. Protobuf messages MUST be encoded in binary and MUST have "application/openmetrics-protobuf; version=1.0.0" as their content type. All payloads MUST be a single binary-encoded MetricSet message, as defined by the OpenMetrics protobuf schema. The protobuf format MUST follow the proto3 version of the protocol buffer language. There is also a requirement for string usage and Timestamp usage under this section. A protobuf format Schema is given for the user to familiarize with these requirements in order to maintain the normalization of the protocol buffer.
	Last, Security and other sections are introduced as fourth actors. In this section, user can have their own choice but need to ensure this section is designed outside the Open Metrics. Three different security suggestions are given, and user can choose their own way to implement the security checking, but should not included in the uploaded file to the Open Metrics.
	Failing to reach any part of these actors will not structurally ruin the other parts, but all together ensure a successful and complete Open Metrics project being uploaded to the open resources cloud-native database and library. 


### Actions
This project is developed on the Prometheus and hence followed all the functionality there as a basic, but has additional tightened and cleaned in format and structure.
	As the goal of this project is to reach normalization of data, recourse, library, and protocol buffer for the cloud-native base, All four actors mentioned in the previous section should play their roles.
	When a user uploads a file into Open Metrics, the text format should be checked. exposition" is the top-level token of the ABNF, hence should be checked. Moreover, the other requirements mentioned in the Actors section should be checked for maintainability. Above the over-structural completion, the protobuf format should be ensured on top of that to ensure the functionality and normalization would work.
	The Datatype as one of the actors works as the basement of the entire structure, where all the data should be stored in a certain format to check the normalization, regardless of the local language or requirement at different user’s end. Last, a security check should be done at the user's end in either authentication, authorization, or accounting check. There is no restriction on this field, hence OpenMetrics asked the user not to include this in the project, but outside the OpenMetrics. 
	OpenMatrics itself will perform security in hashing and protecting users' information with a certain encryption rule to ensure data security for all users. However, the other security goals are not reached and are considered out of scope in this project.

### Goals
OpenMetrics is intended to provide telemetry for online systems. It runs over protocols which do not provide hard or soft real time guarantees, so it can not make any real time guarantees itself. Latency and jitter properties of OpenMetrics are as imprecise as the underlying network, operating systems, CPUs, and the like. It is sufficiently accurate for aggregations to be used as a basis for decision-making, but not to reflect individual events. Systems of all sizes should be supported, from applications that receive a few requests an hour up to monitoring bandwidth usage on a 400Gb network port. Aggregation and analysis of transmitted telemetry should be possible over arbitrary time periods. It is intended to transport snapshots of state at the time of data transmission at a regular cadence.
With regards to security, implementors MAY choose to offer authentication, authorization, and accounting; if they so choose, this SHOULD be handled outside of OpenMetrics. All exposer implementations SHOULD be able to secure their HTTP traffic with TLS 1.2 or later. If an exposer implementation does not support encryption, operators SHOULD use reverse proxies, firewalling, and/or ACLs where feasible. Metric exposition should be independent of production services exposed to end users; as such, having a /metrics endpoint on ports like TCP/80, TCP/443, TCP/8080, and TCP/8443 is generally discouraged for publicly exposed services using OpenMetrics.

### Non-goals
Security Considerations are not included in the Open Matrics and need to be implemented at the user end. Users could choose to handle the Security problem with help from authentication, authorization, and accounting, however, all these should be handled outside of the Open Metrics.
	How ingestors discover which exposers exist, and vice-versa, is out of scope and thus not defined in this standard.
	Avoiding misusing of the OpemMatrics from the user end is not a concern in this project as well. Moreover, this project is not designed to negotiate the conflict between the rule included here and the ABNF section. If there is a conflict, the user should check the ABNF sections as first priority and then follow the rest of the additional rules that OpenMatrics designed. 
	The use of Data type is assumed to be understood by the user, and the user has the obligation to follow the usage there. When using data type, players are required to read and follow the instructions on Open Matrics, and when including the Port Allocations, users are required to follow the structure with these rules published by OpenMetric. It is not OpenMetrics’s goal to convert any type of user input into this format.


## Self-assessment use

In this project, users will be able to be exporter or uploaders to access or upload the information and libraries into the cloud-native resources. This project is based on the Prometheus and will use the ABNF requirements. If any of the requirements on Open Metrics conflict with the ABNF requirements, users should take the ABNF requirements as first priority. 

	To find detailed requirements for users under the Open Metrics, please refer to {https://github.com/OpenObservability/OpenMetrics/blob/main/specification/OpenMetrics.md } In this documentation, users will be able to check all the rules and requirements. Moreover, Default port allocations have a requirement for users as well, users can refer to {https://github.com/prometheus/prometheus/wiki/Default-port-allocations } for detailed instructions on each default port allocation instruction.


## Security functions and features

Security in this project includes two different parts: within OpenMetrics and outside OpenMetrics. Users need to exclude the Security management outside the OpenMetrics, and exporters need to make decisions and develop their own way of designing the Security check.

For outside OpenMetrics Security, implementors MAY choose to offer authentication, authorization, and accounting; if they so choose, this SHOULD be handled outside of OpenMetrics.

All exposer implementations SHOULD be able to secure their HTTP traffic with TLS 1.2 or later. If an exposer implementation does not support encryption, operators SHOULD use reverse proxies, firewalling, and/or ACLs where feasible.

Metric exposition should be independent of production services exposed to end users; as such, having a /metrics endpoint on ports like TCP/80, TCP/443, TCP/8080, and TCP/8443 is generally discouraged for publicly exposed services using OpenMetrics.

For the Security functions within OpenMetrics, OpenMetrics will protect user information with an unknown hash function. All resources other than user information are open resources and allow exporters and uploaders to create cloud-native resources.

Critically, this project did well at protecting the information by not publicizing the security management resource they are using for user information and stored information. However, this also creates a lack of security on the other hand, since it is not open to the public, it is hard to frequently update their security protections.


## Project compliance

* Compliance.  List any security standards or sub-sections the project is
  already documented as meeting (PCI-DSS, COBIT, ISO, GDPR, etc.).

## Secure development practices

* Development Pipeline.  A description of the testing and assessment processes that
  the software undergoes as it is developed and built. Be sure to include specific
information such as if contributors are required to sign commits, if any container
images immutable and signed, how many reviewers before merging, any automated checks for
vulnerabilities, etc.
* Communication Channels. Reference where you document how to reach your team or
  describe in corresponding section.
  * Internal. How do team members communicate with each other?
  * Inbound. How do users or prospective users communicate with the team?
  * Outbound. How do you communicate with your users? (e.g. flibble-announce@
    mailing list)
* Ecosystem. How does your software fit into the cloud native ecosystem?  (e.g.
  Flibber is integrated with both Flocker and Noodles which covers
virtualization for 80% of cloud users. So, our small number of "users" actually
represents very wide usage across the ecosystem since every virtual instance uses
Flibber encryption by default.)

## Security issue resolution

* Responsible Disclosures Process. A outline of the project's responsible
  disclosures process should suspected security issues, incidents, or
vulnerabilities be discovered both external and internal to the project. The
outline should discuss communication methods/strategies.
  * Vulnerability Response Process. Who is responsible for responding to a
    report. What is the reporting process? How would you respond?
* Incident Response. A description of the defined procedures for triage,
  confirmation, notification of vulnerability or security incident, and
patching/update availability.

## Appendix

* Known Issues Over Time. List or summarize statistics of past vulnerabilities
  with links. If none have been reported, provide data, if any, about your track
record in catching issues in code review or automated testing.
* [CII Best Practices](https://www.coreinfrastructure.org/programs/best-practices-program/).
  Best Practices. A brief discussion of where the project is at
  with respect to CII best practices and what it would need to
  achieve the badge.
* Case Studies. Provide context for reviewers by detailing 2-3 scenarios of
  real-world use cases.
* Related Projects / Vendors. Reflect on times prospective users have asked
  about the differences between your project and projectX. Reviewers will have
the same question.
