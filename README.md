# MIRROR Spaces Framework
The MIRROR Spaces Framework (MSF) is a middleware created to enable interoperability between the applications developed in the MIRROR project.

## MIRROR Spaces Concept
It realizes the MIRROR Spaces concept, which specifies a way how users can share information in a controlled and secure environment.

Spaces can be seen as virtual room where users can exchange data in real-time, store and restore data, and communicate with each other. The spaces are provided by the MSF and can be accessed by applications on behalf of the users. As privacy is highly important within MIRROR, an access model with user authentication and authorization is core element of both the spaces concept and its implementation.

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

----

© The MIRROR Project - Co-Funded by EC IST FP7