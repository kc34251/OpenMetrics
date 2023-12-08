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
| SBOM | No SBOM, OpenMetrics is a specification project only and has basically no software |
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

In Open Metrics, four different actors are introduced. To structure the information better, several metric types are also introduced to help the understanding.

First, the Datatype is introduced and should be ensured when implementing under the open metrics environment. For example, the values, boolean, and many other data types that you can see in other languages should follow a certain included method, not based on your local requirements. Failing to follow this Data type requirement will result in having an exception that you are using an invalid datatype, which will ruin the compile process in transmitting.

Second, a text format is introduced as the second actor in order to maintain the running capacity and text normalizing. With the requirement, UTF-8 MUST be used. Byte order markers (BOMs) MUST NOT be used. Each structure, for example, the Escaping and Numbers, should also follow the Open Matrics requirement in order to ensure text normalization. There is also a text format requirement in using Metrics types and this should be ensured as well.

Third, the Protobuf format is also required as the third actors of this project. Protobuf messages MUST be encoded in binary and MUST have "application/openmetrics-protobuf; version=1.0.0" as their content type. All payloads MUST be a single binary-encoded MetricSet message, as defined by the OpenMetrics protobuf schema. The protobuf format MUST follow the proto3 version of the protocol buffer language. There is also a requirement for string usage and Timestamp usage under this section. A protobuf format Schema is given for the user to familiarize with these requirements in order to maintain the normalization of the protocol buffer.

Last, Security and other sections are introduced as fourth actors. In this section, user can have their own choice but need to ensure this section is designed outside the Open Metrics. Three different security suggestions are given, and user can choose their own way to implement the security checking, but should not included in the uploaded file to the Open Metrics.

Failing to reach any part of these actors will not structurally ruin the other parts, but all together ensure a successful and complete Open Metrics project being uploaded to the open resources cloud-native database and library. 

### Actions

This project is developed on Prometheus and hence follows all the functionality there as a basic, but has additional tightened and cleaned format and structure.

As the goal of this project is to reach normalization of data, recourse, library, and protocol buffer for the cloud-native base, All four actors mentioned in the previous section should play their roles.

When a user uploads a file into Open Metrics, the text format should be checked. "exposition" is the top-level token of the ABNF, hence should be checked. Moreover, the other requirements mentioned in the Actors section should be checked for maintainability. Above the over-structural completion, the protobuf format should be ensured on top of that to ensure the functionality and normalization would work.

The Datatype as one of the actors works as the basement of the entire structure, where all the data should be stored in a certain format to check the normalization, regardless of the local language or requirement at different user’s end. Last, a security check should be done at the user's end in either authentication, authorization, or accounting check. There is no restriction on this field, hence OpenMetrics asked the user not to include this in the project, but outside the OpenMetrics. 

### Goals

OpenMetrics is built on Prometheus, aiming to normalize the resources for users with publishing detail restrictions in format. OpenMetrics is intended to provide telemetry for online systems. It runs over protocols which do not provide hard or soft real time guarantees, so it can not make any real time guarantees itself. Latency and jitter properties of OpenMetrics are as imprecise as the underlying network, operating systems, CPUs, and the like. It is sufficiently accurate for aggregations to be used as a basis for decision-making, but not to reflect individual events. Systems of all sizes should be supported, from applications that receive a few requests an hour up to monitoring bandwidth usage on a 400Gb network port. Aggregation and analysis of transmitted telemetry should be possible over arbitrary time periods. It is intended to transport snapshots of state at the time of data transmission at a regular cadence.

With regards to security, implementors MAY choose to offer authentication, authorization, and accounting; if they so choose, this SHOULD be handled outside of OpenMetrics. All exposer implementations SHOULD be able to secure their HTTP traffic with TLS 1.2 or later. If an exposer implementation does not support encryption, operators SHOULD use reverse proxies, firewalling, and/or ACLs where feasible. Metric exposition should be independent of production services exposed to end users; as such, having a /metrics endpoint on ports like TCP/80, TCP/443, TCP/8080, and TCP/8443 is generally discouraged for publicly exposed services using OpenMetrics.

### Non-goals

Security Considerations are not included in the Open Metrics and need to be implemented at the user end. Users could choose to handle the Security problem with help from authentication, authorization, and accounting, however, all these should be handled outside of the Open Metrics. How ingestors discover which exposers exist, and vice-versa, is out of scope and thus not defined in this standard.

Avoiding misusing of the Open Metrics from the user end is not a concern in this project as well. Moreover, this project is not designed to negotiate the conflict between the rule included here and the ABNF section. If there is a conflict, the user should check the ABNF sections as first priority and then follow the rest of the additional rules that Open Metrics designed. 

The use of Data type is assumed to be understood by the user, and the user has the obligation to follow the usage there. When using data type, players are required to read and follow the instructions on Open Matrics, and when including the Port Allocations, users are required to follow the structure with these rules published by Open Metrics. It is not Open Metrics’ goal to convert any type of user input into this format.


## Self-assessment use

This self-assessment is created by the OpenMetrics team to perform an internal analysis of the project's security. It is not intended to provide a security audit of OpenMetrics, or function as an independent assessment or attestation of OpenMetrics's security health.

This document serves to provide OpenMetrics users with an initial understanding of OpenMetrics's security, where to find existing security documentation, OpenMetrics plans for security, and general overview of OpenMetrics security practices, both for development of OpenMetrics as well as security of OpenMetrics.

This document provides the CNCF TAG-Security with an initial understanding of OpenMetrics to assist in a joint-assessment, necessary for projects under incubation. Taken together, this document and the joint-assessment serve as a cornerstone for if and when OpenMetrics seeks graduation and is preparing for a security audit.

In this project, Users can access the cloud-native resource by downloading as exporters or posting as uploader in the libraries. This project is based on the Prometheus and will use the ABNF requirements. If any of the requirements on Open Metrics conflict with the ABNF requirements, users should take the ABNF requirements as first priority. 

To find detailed requirements for users under the Open Metrics, please refer to https://github.com/OpenObservability/OpenMetrics/blob/main/specification/OpenMetrics.md. In this documentation, users will be able to check all the rules and requirements. Moreover, Default port allocations have a requirement for users as well, users can refer to https://github.com/prometheus/prometheus/wiki/Default-port-allocations  for detailed instructions on each default port allocation instruction.

### STRIDE Threat Modeling for OpenMetrics:

#### 1. Spoofing
* An external entity may try to gain access to data metrics by spoofing or stealing credentials/IP and as a result, access is given to the wrong user.

Attack Options: Password theft by phishing, Password crashing by brute force, IP spoofing
	
##### Mitigation:
* Utilizing API tokens, client certificates, or other secure authentication methods.
* Implementing secure authentication mechanisms for metric producers and consumers (exporters and ingesters).

#### 2. Tampering
* Data flowing from endpoints can be tampered with in transit which can lead to corruption of metrics.
* Metric definitions can be exposed to unauthorized alteration which could result in misinterpretation or misrepresentation of data.

##### Mitigation:
* Use hashes or data signatures for data validation and tamper detection. 

#### 3. Repudiation
* Attackers may cover up any evidence of unauthorized actions by altering data logs or information logs.

##### Mitigation:
* Audit and logging mechanisms to track all system activity and suspicious activity.

#### 4. Information Disclosure
* Improperly handling queries may allow an attacker to gain access to information that is not intended for disclosure. E.g. Querying for one metric returns all metrics which are then filtered on the frontend. This data flowing across the access request and response may be sniffed by an attacker.

##### Mitigation:
* Encryption of sensitive data and access control mechanisms to restrict data accessibility.

#### 5. Denial of Service
* An external agent can make so many requests for metrics that it can overwhelm the web server and deny normal user access.
* An attacker can try to overwhelm the data storage by tampering with metric formats to try and store more data than the system can handle. 

##### Mitigation:
* Load balancing and traffic filtering systems to mitigate traffic spikes and spread the load evenly.
* Data tracking mechanisms to track suspicious spikes in data storage.

#### 6. Elevation of Privilege
* A user may be able to gain elevated privileges beyond their normal role and modify metrics that they should not have been able to modify

##### Mitigation:
* Implement the principle of least privilege and role-based access control to limit the privileges of users. 

## Security functions and features

Users need to exclude the Security management outside the OpenMetrics, and exporters need to make decisions and develop their own way of designing the Security check.

For outside OpenMetrics Security, implementors MAY choose to offer authentication, authorization, and accounting; if they so choose, this SHOULD be handled outside of OpenMetrics.

All exposer implementations SHOULD be able to secure their HTTP traffic with TLS 1.2 or later. If an exposer implementation does not support encryption, operators SHOULD use reverse proxies, firewalling, and/or ACLs where feasible.

Metric exposition should be independent of production services exposed to end users; as such, having a /metrics endpoint on ports like TCP/80, TCP/443, TCP/8080, and TCP/8443 is generally discouraged for publicly exposed services using OpenMetrics.

Critically, this project did well at protecting the information by not publicizing the security management resource they are using for user information and stored information. However, this also creates a lack of security on the other hand, since it is not open to the public, it is hard to frequently update their security protections.


## Project compliance

The OpenMetrics project has not been documented as meeting any security standards, however, the OpenMetrics project advises users to handle encryption and security outside of OpenMetrics and to use reverse proxies, firewalling, and/or ACLs where feasible.

## Secure development practices

In regards to the development pipeline, there is a CircleCI pipline that validates the protocol buffer specification and the validity of markdown files. Contributors are also required to sign off commits with DCO. Additionally there is an extensive list of testing scripts that are available which shows that the software has been tested in some form.   

In terms of communication, the project has not documented how team members communicate with each other, but they have available meetings notes on their Github and post updates on their Github to communicate with their users as well as a slack channel in the CNCF slack that allows users to ask questions about the OpenMetrics project. In addition, they have stated in the past that they work on the project by consensus through bi-weekly VC meetings. The project also has a mailing list which you can sign up for to receive updates on the project. They have also listed their team members on their Github page which can allow users or prospective users to reach out to the team.

The OpenMetrics project integrates into the cloud native ecosystem by specifying today's de-facto standard for transmitting cloud-native metrics at scale, with support for both text representation and Protocol Buffers and brings it into an Internet Engineering Task Force (IETF) standard, however, this IETF draft has expired. Additionally, the OpenMetrics project has been adopted by Everquote, GitLab, Grafana Labs as well as SoundCloud.

## Security issue resolution

OpenMetrics has an issue reporting in place in their GitHub repository at https://github.com/OpenObservability/OpenMetrics/issues, which can be used to report security issues, incidents, or vulnerabilities pertaining to both external and internal to the project. OpenMetrics also has a mailing list at https://groups.google.com/g/openmetrics where discussions regarding new updates, other project discussions, and developments are done regarding the project. Vulnerabilities and other issues can be reported in the project’s GitHub repository and they are either being resolved by the project maintainers or the contributors. However, there is no process or plan for for security issues or vulnerabilities as OpenMetrics does not produce any running software. The "releases" of the project are a website and related documentation. 

## Appendix

* The list of known issues over time for OpenMetrics can be found at https://github.com/OpenObservability/OpenMetrics/issues?q=is%3Aissue+is%3Aclosed. There had been no specific source of information regarding the vulnerabilities found in OpenMetrics.
* OpenMetrics does not have an OpenSSF best practices badge and its progress is also not being tracked OpenSSF best practices website. It has a stable website but doesn’t clearly state how to obtain and provide feedback, and doesn’t state other information regarding the contribution process. The project haven’t clearly listed a documented roadmap of what the project intends to do and not to do for atleast the next year. Other criteria that are required for OpenMetrics to get an OpenSSF best practices badge are listed here https://github.com/coreinfrastructure/best-practices-badge#silver and OpenMetrics doesn’t seem to follow most of them.
* Specific case studies of using OpenMetrics are not provided online. The goal of OpenMetrics is to standardize the format in which exporters and ingestors handle data. OpenMetrics is primarily supported by Prometheus. The alternatives to Prometheus are Graphite, InfluxDB, OpenTSDB, Nagios and Sensu, these have their own Data Models. So, taking an example, users who use Prometheus need to convert their data in one Data Model format to another to fit the requirements of other softwares, instead OpenMetrics is trying to define a standardized format which will be helpful to interchange monitoring softwares without changing the Data Model.
* Every other cloud-native monitoring system has its own defined Data Model and there is no sufficient information present regarding the standard formats proposed alternative to OpenMetrics.
