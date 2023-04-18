---
cover: ../.gitbook/assets/Dark Banner_Additional Resourcesv2.png
coverY: 0
---

# SPV & Bitcoin Network

In a Mandala topology of the Bitcoin Network, the Core network is formed of Mining Nodes which are defined as Nodes in the Bitcoin protocol. On top of the core hyperconnected small world network, there will be layers which are formed by lightweight systems which not only store the Block Headers from the blockchain but also connect to one or more core nodes.

These lightweight systems are defined in this toolbox as LiteClient. LiteClient Applications will function as Layer 2 and Layer 3 nodes (shown in the diagram below as Bitcoin Service Layer and Bitcoin Client Network) which still connect to the Peer network in the Blockchain, but do not perform any mining function. LiteClient will only store block headers from the blockchain and will track all chain tips of the bitcoin network at any point in time.

In conjunction with a wallet (PayD), Payment Web Server, Communication Peer Channels server, mAPI and PayMail server, Header SV provides a full featured payment node which can be offered as a BSL layered Bitcoin based payment wallet service. Alternatively, businesses can choose to run this minimal infrastructure and build highly integrated applications with these components.

The LiteClient project will initially deliver the end-to-end SPV Application's Bitcoin lightweight infrastructure which can enable any business application to connect and utilise Bitcoin Blockchain's Payment and Data management capabilities.

An overview of Bitcoin Layered network is shown in the diagram below. It also describes the ancestry node for Blockchain integration with its capabilities. Currently, the LiteClient reference implementation supports this complete description except the storing of the application specific local data which will be provided in the successive release.

<figure><img src="../.gitbook/assets/SPV Architecture for Application DevelopmentDARK.png" alt=""><figcaption><p>SPV Architecture for Application Development</p></figcaption></figure>

In the above diagram, layers are created in the overall bitcoin network and its user network. We currently have a stable core network which is the Peer network of miners and they perform Proof of Work (POW) consensus for verification, clearing and settlement of transactions submitted in the network. The next layer is about users of this core network which is described as the Bitcoin Service layer and Bitcoin Client network layer. They can also be a single entity performing both functions.
