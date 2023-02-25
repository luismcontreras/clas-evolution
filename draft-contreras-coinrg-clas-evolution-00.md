---
title: " An Evolution of Cooperating Layered Architecture for SDN (CLAS) for Compute and Data Awareness"
abbrev: "CLAS Evolution"
docname: draft-contreras-coinrg-clas-evolution-00
category: info

ipr: trust200902
area: General
workgroup: COINRG
keyword: Internet-Draft

stand_alone: yes
smart_quotes: no
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: L.M. Contreras
    name: Luis M. Contreras
    organization: Telefonica
    email: luismiguel.contrerasmurillo@telefonica.com
	
 -
    ins: M. Boucadair
    name: Mohamed Boucadair
    organization: Orange
    email: mohamed.boucadair@orange.com
	
 -
    ins: D. Lopez
    name: Diego R. Lopez
    organization: Telefonica
    email: diego.r.lopez@telefonica.com
	
 -
    ins: C.J. Bernardos
    name: Carlos J. Bernardos
    organization: Universidad Carlos III de Madrid
    email: cjbc@it.uc3m.es

normative:

informative:

  TMV:
    title: Service performance measurement methods over 5G experimental networks
    date: 5G-PPP TMV white paper, May 2021


--- abstract

This document proposes an extension to the Cooperating Layered Architecture for Software-Defined Networking (SDN) by including compute resources and 
data processing.


--- middle

# Introduction

Current telecommunication networks are evolving towards a tight integration of interconnected compute environments, offering capabilities for the 
instantiation of virtualized network functions interworking with physical variants of other network functions, altogether used to build and deliver services.

Moreover, network operations are complementing the capabilities of automation (e.g., {{?RFC8969}}) and programmability (e.g., {{?RFC7149}}{{?RFC7426}}) with the introduction of Artificial Intelligence 
(AI) and Machine Learning (ML) techniques to facilitate informed decisions as well as predictive behaviors enabling consistent closed loop automation.

It is then necessary to provide a network management framework that could incorporate these technical components, structuring the different 
concerns (i.e., connectivity, processing and data analysis) and the interaction among components operating the network. 
Existing approaches, e.g. {{?RFC8969}} only focus on the networking (i.e., connectivity) part without consideration of both compute domain and 
data analysis.  

This document describes an evolution of the Cooperating Layered Architecture for Software-Defined Networking (CLAS) 
{{?RFC8597}} to include the aforementioned aspects into the architecture.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Cooperating Layered Architecture for Software-Defined Networking (CLAS)

{{?RFC8597}} describes an SDN architecture structured in two different strata, namely Service Stratum and Transport Stratum.  On one hand, the Service 
Stratum contains the functions related to the provision of services and the capabilities offered to external applications. On the other hand, the 
Transport Stratum comprises the functions focused on the transfer of data between the communication endpoints (e.g., between end-user devices, 
between two service gateways, etc.).
   
Each of the strata is structured in different planes, as follows:
   
* The Control plane, which centralizes the control functions of each stratum and directly controls the corresponding resources.

* The Management plane, logically centralizing the management functions for each stratum, including the management of the control and resource planes.

* The Resource plane, that comprises the resources for either the transport or the service functions.  

Figure 1 illustrates the original CLAS architecture.

                                          Applications
                                               /\
                                               ||
                                               ||
         +-------------------------------------||-------------+
         | Service Stratum                     ||             |
         |                                     \/             |
         |                       ...........................  |
         |                       . SDN Intelligence        .  |
         |                       .                         .  |
         |  +--------------+     .        +--------------+ .  |
         |  | Resource Pl. |     .        |   Mgmt. Pl.  | .  |
         |  |              |<===>.  +--------------+     | .  |
         |  |              |     .  |  Control Pl. |     | .  |
         |  +--------------+     .  |              |-----+ .  |
         |                       .  |              |       .  |
         |                       .  +--------------+       .  |
         |                       ...........................  |
         |                                     /\             |
         |                                     ||             |
         +-------------------------------------||-------------+
                                               ||    Standard
                                            -- || --    API
                                               ||
         +-------------------------------------||-------------+
         | Transport Stratum                   ||             |
         |                                     \/             |
         |                       ...........................  |
         |                       . SDN Intelligence        .  |
         |                       .                         .  |
         |  +--------------+     .        +--------------+ .  |
         |  | Resource Pl. |     .        |   Mgmt. Pl.  | .  |
         |  |              |<===>.  +--------------+     | .  |
         |  |              |     .  |  Control Pl. |     | .  |
         |  +--------------+     .  |              |-----+ .  |
         |                       .  |              |       .  |
         |                       .  +--------------+       .  |
         |                       ...........................  |
         |                                                    |
         |                                                    |
         +----------------------------------------------------+
		 
     Figure 1: Cooperating Layered Architecture for SDN {{?RFC8597}}


# Augmentation of CLAS with Compute and Data Awareness

The CLAS architecture was initially conceived from the perspective of exploiting the advantages of network programmability in operational networks.

The evolution of current telecommunication services and networks are, however, introducing new aspects:

* Considerations of distributed computing capabilities attached to different points in the network, intended for hosting a variety of services and 
applications usually in a virtualized manner (e.g., {{?I-D.contreras-alto-service-edge}}).

* Introduction of Artificial Intelligence (AI) and Machine Learning (ML) techniques in order to improve operations by means of closed loop 
automation (e.g., {{?I-D.francois-nmrg-ai-challenges}}).

With that in mind, this memo proposes augmentations to the original CLAS architecure by adding the aforementioned aspects.


## Compute Stratum

The CLAS architecture is extended by adding a new stratum, named Compute Stratum.  This stratum contains
the control, management, and resource planes related to the computing aspects.  This additional 
stratum cooperates with the other two in order to facilitate the overall service provision in the network.

With this addition, and in order to be more explicit in the strata scope, the previously named Transport Stratum is renamed as Connectivity Stratum, 
representing the fact that this stratum responsibility is focused on the overall connectivity supporting the other two strata in the architecture.

## Learning Plane

A further extension to the original CLAS architecture is related to the need of collecting, processing and sharing relevant data from each of the 
considered strata.  With that purpose a Learning Plane is proposed to complement the already existing planes per stratum.

The learning plane will be in charge of handling the data specificities of each stratum.  Thus, the learning plane in the Service Stratum 
is focused on data relevant to the service as defined by the application or service owner, usually in terms of service key performance indicators 
(KPI) {{TMV}}.  Then, the learning plane in the compute stratum concentrates on data related to the computing capabilities in use (e.g., CPU load, RAM 
usage, storage utilization, etc) [OpenStack].  Finally, the learning plane in the network stratum is in charge of handling the monitoring and 
telemetry information obtained from the network (e.g., {{?I-D.ietf-opsawg-service-assurance-yang}}).

## Extended CLAS architecture

Figure 2 presents the augmentation proposed showing the relationship among strata.

                                         Applications
                                               /\
                                               ||
         +-------------------------------------||-------------+
         | Service Stratum                     ||             |
         |                                     \/             |
         |  +--------------+     ...........................  |
         |  | Learning Pl. |     . SDN Intelligence        .  |
         |  |              |<===>.                         .  |
         |  +-----/\-------+     .        +--------------+ .  |
         |        ||             .        |   Mgmt. Pl.  | .  |
         |        ||             .  +--------------+     | .  |
         |  +-----\/-------+     .  |  Control Pl. |-----+ .  |
         |  | Resource Pl. |     .  |              |       .  |
         |  |              |<===>.  +--------------+       .  |
         |  +--------------+     ...........................  |
         |                                /\             /\   |
         |                                ||             ||   |
         +--------------------------------||-------------||---+
                          Standard API -- || --          ||
         +--------------------------------||-----+       ||
         | Compute Stratum                ||     |       ||
         |                                \/     |       ||
         |  +----------+    ...................  |       ||
         |  | Learning |    . SDN             .  |  Std. ||
         |  | Plane    |<==>. Intelligence    .  |  API  ||
         |  +-----/\---+    .    +----------+ .  |    -- || --
         |        ||        .    | Mgmt. Pl.| .  |       ||
         |        ||        .  +----------+ | .  |       ||
         |  +-----\/---+    .  | Control  |-+ .  |       ||
         |  | Resource |    .  | Plane    |   .  |       ||
         |  | Plane    |<==>.  +----------+   .  |       ||
         |  +----------+    ...................  |       ||
         +----------------------------------/\---+       ||
                            Standard API -- || --        ||
                        +-------------------||-----------||-----+
                        | Connectivity      ||           ||     |
                        | Stratum           ||           ||     |
                        |                   \/           \/     |
                        |  +----------+    ...................  |
                        |  | Learning |    . SDN             .  |
                        |  | Plane    |<==>. Intelligence    .  |
                        |  +-----/\---+    .    +----------+ .  |
                        |        ||        .    | Mgmt. Pl.| .  |
                        |        ||        .  +----------+ | .  |
                        |  +-----\/---+    .  | Control  |-+ .  |
                        |  | Resource |    .  | Plane    |   .  |
                        |  | Plane    |<==>.  +----------+   .  |
                        |  +----------+    ...................  |
                        +---------------------------------------+

                   Figure 2: Extended CLAS architecture
				   
# Discusion on research aspects of the proposed architecture


## Discusion related to the Compute Stratum

The inclusion of the Compute Stratum allows the extension of the resource layer/plane in a manner that the network (i.e., including 
processing capabilities adn the associated connectivity) can be programmed consistently in an integrated way. This is very relevant when 
evolving to network architectures pursuing the could-edge continuum, even considering the extension to the very extreme edge. 

Important to note. the aforementioned cloud-edge continuum could be potentially constituted by resources from multiple administrative 
domains. Enabling the management of multiple heterogeneous domains in a so-called "frictionless" manner is the necessary to be explored. 


## Discusion related to the Learning Plane

One os the aspects to investigate is the application of AI to network management and control. There are multiple flows to consider:

* Data in the closed loop such as the monitoring/telemetry flows from network to AI as well as action/control from AI to network
* Flows related to AI behavior (policies/intents) as defined by the network admins towards the AI
* Feedback (i.e., predictions, suggedtesd actions, etc) from AI to network administrators
* Flows facilitating the cooperation among distinct Learning Planes, implying knowledge sharing among different segments, and knowledge 
aggregation at different strata of control. 

A potential way to follow is the definition of a common, model-based, approach, also defining a recursive structure that could become a 
generalization of the CLAS model.



# TODO for next versions of this document

This version is a work-in-progress.  Next versions of the document will address some further aspects such as:

* Communication between strata (and planes).

* Deployment scenarios (including legacy ones).

* Potential use cases (specially in alignment with on-going activities in COINRG / NMRG).	



# Security Considerations

   Same security considerations as reflected in {{?RFC8597}} with regards
   to the strata architecture apply also here.

   Apart from that, the introduction of the Learning plane on the data
   management imposes additional security concerns.  
   
   (TODO: elaborate on data-related security issues).


# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

This work has been partially funded by the European Union under Horizon Europe projects NEMO (NExt generation Meta Operating system) grant number 
101070118, and CODECO (COgnitive, Decentralised Edge-Cloud Orchestration), grant number 101092696.