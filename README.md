# MIRROR Spaces Framework
The MIRROR Spaces Framework (MSF) is a middleware created to enable interoperability between the applications developed in the MIRROR project.

## MIRROR Spaces Concept
The MSF implements the MIRROR Spaces concept, which specifies a way how users can share information in a controlled and secure environment. Spaces can be seen as virtual room where users can exchange data in real-time, store and restore data, and communicate with each other. The spaces are provided by the MSF and can be accessed by applications on behalf of the users. As privacy is highly important within MIRROR, an access model with user authentication and authorization is core element of both the spaces concept and its implementation.

![Collaboration with MIRROR Spaces][1]

The specific requirements of individual, collaborative, and organizational environments are supported by different types of spaces:

- **Private Spaces** are unique for each user and provide a maximum level of privacy.
- **Team Spaces** can be shared between multiple users to support both inter-application and inter-user communication.
- **Organizational Spaces** add control over the data exchanged by applying data object filtering and validation.

## Implementation

The MIRROR Spaces Framework is built on top of open technologies and standards for data exchange. Being built on top of the *Extensible Messaging and Presence Protocol (XMPP)*, the MSF can be integrated in applications of nearly any platform, including embedded devices, mobile platforms, and enterprise service applications.

The MIRROR Spaces Framework design aims for a very high modularity. Clear interface specifications allow the replacement of single components, e.g., to improve performance in more sophisticated implementations (*Maintainability*). Except for the Spaces Service as core component, all other services and tools can be deployed on demand (*Customizability*). It is also easy to add additional services and tools to the MSF (*Extensibility*).

The MSF is composed by the following components:

- A **XMPP server** for real-time data exchange, taking care of user management and authentication, encryption, and component handling. Multiple servers can be connected in a server federation.
- Several framework **services** providing the features specified in the MIRROR Spaces concept, for example space handling and data persistence.
- **Tools** for users to handle the framework services, e.g., to manage users and spaces.
- **Software Development Kits (SDKs)** for several platforms to ease the usage of the framework for application developers.


## Components

![Components of the MSF][2]

### Server
Central component of the MIRROR Spaces Framework is an XMPP server. The core services (see below) are implemented as plugins for the [Openfire XMPP server][3]. It is an Java-based server released under the Apache License 2.0.

### Services
Services extend the features provided by the XMPP server.

#### Spaces Service
The MIRROR Spaces Service is responsible for the management of spaces, and therefore a mandatory component of the MIRROR Spaces Framework. It is realized as XMPP component and implemented as plugin for the Openfire XMPP server.

[↪ Details][4]

#### Persistence Service
The MIRROR Persistence Service adds an sophisticated persistence layer to the MIRROR Spaces Framework. The service is registered as component of the XMPP domain and therefore acts on the same level as the MIRROR Spaces Service. Whilst the Spaces Service is mandatory within the MIRROR Spaces Framework (MSF), the Persistence Service is optional. In its current implementation, the service is realized as plug-in for the Openfire XMPP server.

[↪ Details][5]

#### File Service
One of the disadvantages of the XML based communication approach is the handling of binary data files. To perform and in-band data transfer, the data has to be encoded as string (e.g. using a Base64 encoding schema) and embedded in the message.

Using existing approaches to this issue, two alternatives offer themselves: Using XEP-0096 “SI File Transfer” or following an out-band approach to transfer the data. The first solution has several benefits: It is an official draft standard for XMPP, transfers the data in-band using binary streams, and is already supported by the Openfire server used in the MSF. The reason the XEP cannot be applied to our domain is based on a conceptual incompatibility: A file transfer is always performed between two clients, whilst a data object published on a space is usually shared with multiple clients. Sequentially transferring the file from the sender to all receivers would create even more load than the naive approach.

In an out-band approach, the data is stored externally using a protocol capable of transferring binary data, e.g., on a web server using HTTP. The address is then embedded in the data object so the receivers can request the file from there. This solution solves both issues addressed above: No encoding is required and the data objects contain only the address information, which is of small and almost non-varying size. The major drawback and disqualifier is the loss of control over data: The MIRROR spaces access model (MSAM) is not applied, so being aware of an address or the address schema, the files can be accessed by non-authorized persons.

This is taken up by the MIRROR File Service. The service provides a repository for files using XMPP user authentication and the MSAM authorization model. To do so, the service implements the standardized WebDAV (Web Distributed Authoring and Versioning) protocol, which sits on top of HTTP. To apply the MSAM, the service relies on HTTP BASIC authentication mechanism, i.e., the client has to pass the user credentials with each request.  The service checks if the user can be authenticated by the XMPP server and is authorized to access the space.

↪ *The service will be available on GitHub with the next release.*

### Tools
Tools provide user interfaces to handle the features provided by the MIRROR Spaces Framework.

#### Space Manager
The MIRROR Space Manager is a web application allowing users to manage their spaces separately from the MIRROR applications. As any application connected to the MSF acts on behalf of a user and has access to all spaces the user is member of, this tool enables a shifting the space management from the application to the user level

[↪ Details][6]

### SDKs
One of the reasons for choosing XMPP as base technology for the framework was the availability of client libraries for nearly any development platform. The libraries serve two major purposes: First they handle the connection to the XMPP network. Then they abstract from the protocol layer for the both the basic XMPP protocol and for supported protocol extensions.

By working directly with the protocol layer by exchanging XML stanzas, all features provided by the MSF can be used with any XMPP client library. But while several features rely on existing protocol extensions supported by most of the libraries (e.g. the user authentication and the publish-subscribe mechanism), the framework services also add their own protocol extensions. This includes the space management and queries addressing the persistence service. Using these protocol extensions enforces a lot of XML processing and a detailed knowledge of the framework API.

To decrease the development effort, we introduced “Spaces Software Development Kits” (Spaces SDKs) for several platforms. The SDKs are used as software libraries (as illustrated below) and provide a high-level API for the MSF protocol extensions. Additionally, they add an abstraction layer to the already existing protocol support by the underlying XMPP libraries. An example for this additional layer is the handling of data objects: We provide a data handler which is registered to one or more spaces in order to retrieve data objects. This handler abstracts the handling of publish-subscribe node, acts as observer for incoming messages, generates (high-level) data objects based on the items received, etc.

![Spaces SDK Concept][7]

Although different platform enforce different coding schemes, we provide a common API for all SDK implementations ([↪ Details][8]). On the one hand, the API specifies the basic design of the SDK. On the other hand, the version of the API reflects the list of MSF features supported by the SDK.  The SDK implementations share the major and minor version of the API. With this approach, it is easy to compare the different implementations in regard to their feature support.

We currently maintain three implementations of the Spaces SDK:
•	An implementation for **Java-based applications**, including both desktop and web application platforms. ([↪ Details][9])
•	An implementation for mobile devices running on **Android** with version 2.2 or higher. ([↪ Details][10])
•	An implementation written in **JavaScript** designed for single-page web-applications running the web browser. ([↪ Details][11])

----

© The MIRROR Project - Co-Funded by EC IST FP7


  [1]: https://raw.github.com/MirrorIP/msf/master/images/spaces-concept.png
  [2]: https://raw.github.com/MirrorIP/msf/master/images/msf-components.png
  [3]: http://www.igniterealtime.org/projects/openfire/
  [4]: https://github.com/MirrorIP/msf-spaces-service
  [5]: https://github.com/MirrorIP/msf-persistence-service
  [6]: https://github.com/MirrorIP/msf-space-manager
  [7]: https://raw.github.com/MirrorIP/msf/master/images/sdk-schema.png
  [8]: https://github.com/MirrorIP/msf-spaces-sdk-api
  [9]: https://github.com/MirrorIP/msf-spaces-sdk-java
  [10]: https://github.com/MirrorIP/msf-spaces-sdk-android
  [11]: https://github.com/MirrorIP/msf-spaces-sdk-javascript
