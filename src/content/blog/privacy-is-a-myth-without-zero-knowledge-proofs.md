---
author: Illya Gerasymchuk
pubDatetime: 2024-05-26T14:14:00Z
title: "Privacy Is A Myth. Unless You're Using Zero-Knowledge Proofs"
postSlug: privacy-is-a-myth-without-zero-knowledge-proofs
featured: false
draft: false
tags:
  - zero-knowledge
  - privacy
description: "Privacy is a fundamental human right, yet digital privacy is a myth. Zero-Knowledge Proofs (ZKPs) turn this myth into reality, by enabling inherently private data sharing through a verifiable computation model. Discover how ZK solutions like zkLocus, zkSafeZones, zkML, zkVM, Merkle Trees, MPC, and homomorphic encryption, enable privacy in all digita products - from search engines and social networks to e-commerce and 24/7 global surveillance, without compromising functionality or individual privacy."
---

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/privacy-with-zero-knowlege-cover.jpeg" alt="Cover image showcasing a serene, retro-futuristic scene representing the theme of Zero-Knowledge Proofs enabling true digital privacy. The image highlights the transition from the myth of digital privacy to the reality achieved through ZKPs." style="max-width:100%; max-height:auto;border:none;">
</div>

Privacy is fundamental human right. Digital privacy is fundamentally a myth. While it's possible to store data privately for personal use, that privacy is foregone the moment that data is shared with someone else. This is true even if encryption is employed. The problem is not in **how** the data is stored & shared, but rather in **what** data is stored & shared.

The good news is that **it's possible to achieve full privacy in the digital realm**. Such a feat requires a a fundamental shift to **what** we define as data apt for sharing. While the solutions to this problem have been conceptualized since the 1970's, they have only became feasable in recent years, thanks to the advancements in computing power, cryptography, and [verifiable computational models](https://illya.sh/blog/posts/zksnark-zkstark-verifiable-computation-model-blockchain/).

Zero-Knowledge Proofs (ZKP) and their protocols like zkSNARKs and zkSTARKs, present the most flexible, versatile and interoperable solution to the data privacy problem. By turning data into a computation, they allow for the representation of data in a manner that inherently preserves its privacy. It's as intutive as it gets: **if the data you are sharing is already private, then there can be no breach of privacy**. It also means that the complexity and the cost of systems operating on that data is greatly reduced. The first solutions of this kind are already emerging.

[zkLocus](https://zklocus.dev) leverages Zero-Knowledge (ZK) to enable fully private geolocation sharing, by enabling users to share their geogrpahical location without revealing their exact coordinates. It does so by cryptographically processing the set of coordinates into a succint proof demonstrating that the user is within a certain geographical area without revealing any additional information about the actual geolocation. This proof is then stored and shared, instead of the raw coordinate data. Deriving its properties from recursive zkSNARKs, this proof is succint, and can generated and verified by anyone on every day computing devices, such as smartphones, laptops, and IoT.

[zkSafeZones](https://zklocus.dev/zkSafeZones/) builds on top of zkLocus and the [Mina Protocol blockchain](https://minaprotocol.com/) to solve a long-standing humanitirian problem of civilian casualties in conflict zones. By enabling civilians to share their geolocation without revealing their exact coordinates, zkSafeZones allows for the creation of safe zones, where civilians can seek refuge without fear of being targeted. By having this data stored privately on a public blockchain, it makes it impossible for offending entities to deny the presence or knowledge of presence of civilians in an geographical area. zkSafeZones also provides a blueprint for the enforcement of the law, such as the International Humanitarian Law (IHL), by automating the execution of policies, penalties, and legislation via smart contracts.

For more information on zkLocus and zkSafeZones you can consult the following resources:

- ⛑️📄 zkSafeZones Whitepaper: [zklocus.dev/zkSafeZones/](https://zklocus.dev/zkSafeZones/)
- 📍📄 zkLocus Whitepaper: [zklocus.dev/whitepaper](https://zklocus.dev/whitepaper)
- 📍🆇 zkLocus Twitter/X: [x.com/zkLocus](https://x.com/zkLocus)
- 📍🔗 zkLocus Homepage: [zklocus.dev](https://zklocus.dev)

Now, let's proceed with a more detailed look at how ZKPs are both, what we need and what we deserve for true digital privacy, as well as look at some promising alternative and complementary approaches.

## The Myth of Digital Privacy

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/digital-privacy-is-a-myth.jpeg" alt="Illustration of a vibrant, futuristic cityscape under constant surveillance, emphasizing the challenges of maintaining digital privacy and reinforcing the concept that digital privacy is a myth." style="max-width:100%; max-height:auto;border:none;">
</div>

As stated in the introduction to this article - **digital privacy is fundamentally a myth**. The core problem lies not in the fact that we share data, but the fact that we share data that is too revealing. Once that data leaves our personal device, it becomes public. Sure, you may think that whoever you are sharing this data with will keep it safeguarded, but in terms of computer security - that data is compromised. It's not a matter of opinion, this is how cryptographically secure systems are designed, with mathematics being the only counterparty to your trust, and subsequently, your privacy. A secret is only a secret, as long as it remains only known and accessible to you.

Of course, there is a simple solution to this problem - let's stop sharing our data 😄. If you permanentely disconnect all of your devices from the internet, remove the SIM card from your smartphone, and physiclly disable all other forms forms of digital communication such as Bluetooth, GSM and NFC, then you can stop reading this article now. However, just because a solution exists, it doesn't mean that it's neither a practical one, nor a desirable one. There are many good reasons for us to share our data with third-parties, such as the improved user experience and a more speedy and automated completion of our goals and tasks.

Search engines, social networks and online shopping platforms are examples of services that we use on a daily basis that require us to share our data with them, by the very nature of their operation. A search engine requires the access to your queries in order to provide you with the most relevant search results. A social network requires the access to the graph of your connections in order to connect people. An online e-commerce platform requires the access to your geolocation in order to provide you with the list of the available products, and the associated costs. All of these services require us to **share our data with them**, and they need the access to this data in order to provide us with the service that we desire.
At the same time, these businesses use the data that you provide them with in order to improve their product, and consequently the quality of the service that they offer. The problem is that this process abdictes us of the control over our data and its privacy.

Zero-Knowledge Proofs can be used to make each one of the aforementioned services fully private, without compromising the quality of the service that they offer, and while enabling them to perform data collection and analysis at the same level that they do today. This enables a disruptive paradigm, where the end-user privacy is preserved while remaining fully compatible with the existing business models and products. In this article we'll see how exacly this can be done, but first, let's construct the mental model shift that is required when opearting with ZKP.

## Zero-Knowledge - Think Different (About Data)

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/zk-data-as-computation.jpeg" alt="Diagram demonstrating how Zero-Knowledge Proofs transform 'Original Data (Private)' into 'Abstracted Data (Public)' through Zero-Knowledge Computation, highlighting the concept of data abstraction for privacy preservation." style="max-width:100%; max-height:auto;border:none;">
</div>

ZKP allow you to create cryptographic assertions about data without revealing the data itself. For example, instead of providing your exact age, you can provide a spectrum of your age, such as "I am between 20 and 30 years old". This assertion can be cryptographically proven to be true, without revealing your exact age. The same principle can be applied in to finance. Instead of revealing your full holdings and bank statements when applying for a loan, you can provide a proof that you have the required amount of funds, without revealing the exact amount. For example, you could demonstrate that you possess **at least 1 million euros** without revealing your full extent of your assets, with that proof being backed by full faith of mathematics. In both cases we share an assertion about the data, instead of the actual data. This assertion is a result of running a computation over the data, and it can be used as inputs for furhter computations, thus allowing to create assertions of arbitrary breadth and complexity. In the processs of this computation, **the original data is abstracted**, and the resulting superset of the data is shared, instead of the original data.

### Data Abstraction

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/zk-data-abstraction.jpeg" alt="Visual representation of data abstraction in Zero-Knowledge Proofs, showing how private data is abstracted into a more general, public form, illustrating the creation of privacy-preserving data assertions." style="max-width:100%; max-height:auto;border:none;">
</div>

Zero-Knowledge computation over data is a form of **data abstraction**, with the resulting proof being a representation of the data that is more general than the original data. This computation takes the original data as input, and outputs a superset of that data. The whole process is performed in full on the user's local device, with the original data remaining private, and only the abstracted representation of it being shared. As a result, the end user retains full custody of their data and control the precise extent to which extent the information about their data is shared.

### zkML - Zero-Knowledge Machine Learning

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/zkML-zero-knowledge-machine-learning.jpeg" alt="Illustration of Zero-Knowledge Machine Learning (zkML), combining AI and Zero-Knowledge Proofs. The image depicts a futuristic, AI-driven environment where data privacy is maintained through verifiable computation." style="max-width:100%; max-height:auto;border:none;">
</div>

zkML is the combination of Artificial Intelligence (AI) and Zero-Knowlede (ZK), which empower the AI systems with [verifiable computation and privacy features](https://illya.sh/blog/posts/zksnark-zkstark-verifiable-computation-model-blockchain/). Machine Learning (ML) is a subset of AI that focuses on the development of algorithms that can learn from and make predictions or decisions based on data. ML involves two primary phases: training, where the model learns patterns from the data, and inference, where the model applies what it has learning during training to perform perdictions on new data. Large Language Models (LLMs), such as OpenAI's GPT, Meta's Llama and Google's Gemini are examples of ML models designed to interact using natural language (i.e. spoken or written human language). Other ML models are designed for other purpuses, such as Artificial Neural Networks (ANNs) for financial analysis.

zkML stands for "Zero-Knowledge Machine Learning", and it refers to the integration of Zero-Knowledge Proofs into the training and/or inference phases of ML. zkML allows for mathematically verifiable inference-based data abstraction with [private inputs to the computaion](https://illya.sh/blog/posts/zk-snarks-recursive-proof-private-intput-visibility/). It allows to prove that a machine learnig model training and/or inference is performed correctl ywithout revealing the model's parameters or the input data (for an LLM, this input data is the _prompt_).

While zkML can be applied to both, training and inference pahses of ML, the current applications focus on the inference (usage) phase, as it's less complex and has more immediate practical applications. The field of ZKPs is both, computationally complex and at its inception in terms of practical use-cases. As we further optimize the algorithms and hardware, we'll see zkML extend to all phases of ML.

In the next section, we'll see the crutial role that zkML will play in enabling complex privacy-preserving systems, and cover how it's possible to have global 24/7 hour video surveillance without compromising the privacy of individuals and groups.

### Merkle Trees

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/zk-privacy-merkle-tree.jpeg" alt="Depiction of how Merkle Trees, combined with Zero-Knowledge Proofs, enable private data operations. The image shows a cryptographic data structure managing data efficiently while preserving privacy through verifiable private data access." style="max-width:100%; max-height:auto;border:none;">
</div>

Merkle Trees and similar constructs like Verkle Trees are cryptographic data structures that allow to store and manage data in an efficient manner with integrity guarantees. When combined with ZK, they allow to create (insert), read, update and delete (CRUD) operaitons on the underlying data, while allowing to keeping the underlying data fully private. The only piece of data that needs to be shared publically is the Merkle Root, which is a hash of all of the underlying data, and does not reveal anything about that data.

Merkle trees already come equipped with a verifiable private data access layer - the R (read) of CRUD, by allowing to prove that a piece of data is contained within the merkle tree. ZKPs add a verifiable computation layer to merkle trees, which enables private and provable modifications, i.e the C (create/insert), U(update/modify) and D(delete) to that data. Together, they enable a data abstraction layer (DAL) that allows to verifiably access and modify a dataset, while maintaining the full privacy of that dataset.

In the next section we will explore how merkle trees can be used to enable fully private data structures and relationships, as well as how those can be leveraged to construct privacy-preserving networks and applications.

## Privacy With Zero-Knowledge In Practice

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/zk-privacy-in-practice.jpeg" alt="Illustration of practical applications of Zero-Knowledge Proofs in preserving digital privacy. The image depicts a high-tech environment where various ZKP-based privacy solutions are implemented, showcasing the feasibility of achieving true digital privacy." style="max-width:100%; max-height:auto;border:none;">
</div>

Throughout this article we have gradually constructed a mental model of how Zero-Knowledge Proofs can be used to preserve privacy in the digital realm, by using a mix of theory and practical examples. However, we didn't cover all of the examples that were mentioned. In an offer to not leave any open loops and futher solidify our understanding, in this section we will describe on how a privacy-preserving search engine, social network and shopping platform can be built using ZK technology. We will keep our discussion at the higher level, by focusing on the architecture and its components. If you are interested in the tehcnical details without mathematics on how Zero-Knowledge Proofs and their [protocols like zkSNARKs and zkSTARKs](https://illya.sh/blog/posts/zksnark-zkstark-verifiable-computation-model-blockchain/) work, you can consult the article that I have written on that topic.

### Privacy-Preserving Search Engine

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/zk-private-search-engine.jpeg" alt="Conceptual image of a privacy-preserving search engine using Zero-Knowledge Proofs. The scene portrays a futuristic search engine interface, emphasizing the abstraction of search queries to maintain user privacy." style="max-width:100%; max-height:auto;border:none;">
</div>

Whenever you use a search engine, you are sharing your search queries with the search engine provider. The search queries is the **data** in question. From the functionality perspective, this data is used to provide the end-user with the most relevant search results. From the business perspective, this data is used to improve the search results, as well as to serve you targeted ads. While both are legitimate use-cases, the problem is that your search queires are shared verbatim with the search engine provider, and they can be used to build a profile on you, identify you, and to track your online activity.

A privacy-preserving search engine can be built by incorporating Zero-Knowledge Proofs into its architecture. In such a system, the search queries are abstracted before being shared with the search engine provider. The search engine provider would not have access to your exact search queries, but rather to the abstracted representation of them. This abstraction is generated locally on your device, by running ZK computation over those search queries. The resulting abstracted representation is then shared with the search engine provider, which will return the matching search results, which would be further filtered on your device, without ever revealing your raw search queries to the search engine provider. This ZK-powered approach is compatible with the existing business and operation models of search engines, while preserving the end-user privacy.

In terms of a more detailed architecture, zkML can be integrated into the process in 3 simple steps:

1. Locally, the user runs a zkML computation over the search query (the **data**) to verifiably abstract it into a set of more general keywords, categories and topics. For example, the search query "best vegan restaurants in Bucharest" can be abstracted into "best healthy food in Germany, Romania, Paris and Lisbon".
2. The abstracted representation of the search query is sent to the search engine provider, which returns the search results for the query.
3. Locally, the user further filters the search results according to their needs. In this case, we're only interested in the vegan restaurants in Bucharest

In this process, neither the location of the user, nor the type of food they are seeking are revealed to the search engine provider. At the same time, the abstracted data can be used to build a privacy-preserving profile of the user, which can be used to improve the search results, serve ads and improve the user experience.

### Privacy-Preserving Social Network

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/zk-private-social-network.jpeg" alt="Visual representation of a privacy-preserving social network enabled by Zero-Knowledge Proofs. The image depicts users interacting within a futuristic social network, highlighting the privacy of connections and shared content." style="max-width:100%; max-height:auto;border:none;">
</div>

We can use the same ideas to construct a privacy-preserving social network, where neither the connections/relationships, nor the content shared by the user are revealed to the social network provider. Connections and relationships are usually represented via a graph, where the nodes are the users, and the edges are the connections between them. These connections can be either uni-directional (followers) or bi-directional (friends). The content shared by the user can be text, images, videos, or other forms of media.

In order to implement the connections in a privacy-preserving manner, we can use a combinations of Zero-Knowledge Proofs and Merkle Trees. Each user would maintain their own Merkle Tree, whose underlying data is the list of their connections. The updates to those Merkle Trees would be performed using Zero-Knowledge Proofs, which would allow to add, remove and update the connections without revealing the exact connections to the social network provider. The social network provider would only have access to the Merkle Root of the Merkle Tree, which would allow them to verify the validity and integrity of the connections (**data**), without revealing the connections (**data**) itself.

The shared content can be implemented using a similar approach, with the added layer of confidentiality through encryption for communications that are meant to remain private to a select set of entities. By employing Multi Party Computation (MPC) techniques it's possible to only reveal the shared content to individuals that share a minimum number of connections with the user. Data abstraction can also be applied to make the involved identities private, by to proving that the user is one of the allowed users in the system, without revealing who they are.

### Privacy-Preserving E-Commerce Platform With zkLocus

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/zk-private-ecommerce-zklocus.jpeg" alt="Illustration of a privacy-preserving e-commerce platform using zkLocus and Zero-Knowledge Proofs. The image shows a vibrant marketplace where users' search queries and shipping information remain private, ensuring secure online shopping." style="max-width:100%; max-height:auto;border:none;">
</div>

Online shopping privacy concerns are a combination of privacy concerns of search engines and social networks, and they can be addressed by using similar approaches to the ones described above. There are two main data points that are need to be kept private:

1. The user's search queries.
2. The user's name and shipping address, for the delivery.

Now let's analyze how we can can keep each one of them private during the process. The search queries can be implemented by using zkML to abstract the search queries, akin to private search engine example. For the latter two points we can leverage zkLocus's support for private and semi-private geolocation sharing, the ability to run arbitrary computation over the zkLocus proofs, as well as the ability to associate metadata with each zkLocus proof. The user's name and shipping address can be kept private, and never revealed to the e-commerce platform by following the setps below:

1. Locally, the user creates a zkLocus proof of the exact geolocation corresponding to their shipping address. This information never leaves the user's local device.
2. There exists a Merkle Tree containing the association between physical addrsses and the corresponding geographical areas. This Merkle Tree can be maintained by the e-commerce platform, and its data must be public.
3. Locally, user associates their name and shipping address as metadata to the zkLocus poof created in step 1. This process of association is mediated by a Zero-Knowledge Proof which asserts that the provided shipping address is indeed within the geographical area corresponding to the proof. For this, a witness for the Merkle Tree from step 2 is provided by the user, and its asserted that the geolocation from step 1 is within the geographical area is defined in the Merkle Tree. The end result of this step is a zkLocus proof of the exact geolocation, alongside the user's name and shipping address as metadata.
4. Locally, the user uses the zkLocus protocol to turn the proof from step 3 into a private geolocaiton proof, by abstracting it into a more general geographical area. This geographical area is represented as an arbitrary combination of polygons which delimit an area in which the geolocation is contained. The user decides how general or specific this area is, and can adjust it according to their needs. For example, they can limit the area to a single city, or expand it to a whole country. The end result is a private geolocation proof, which contains the user's name and shipping address as metadata. It's important to note that at this point the user's name and shipping address are not private. In zkLocus, the metadata is constructed by running SHA-3 over the user's name and shipping address. As such, it will likely be possible to bruteforce the name and the shipping address from the metadata.
5. Locally, the user makes the metadata confidential, by hashing it with a unique (pseudo)random number. This number, which we will refer to as _salt_, is kept secret and only shared later to allow to extract the name and shipping address from the metadata. In this sense, this salt is akin to a symmetric encryption key. The end result of this step is a private zkLocus geolocation proof, which contains the "encrypted" metadata of the user's name and shipping address. The pseudo-random number can be generated using [RandoMina](https://github.com/iluxonchik/randomina), which powers the random number generation in zkLocus.
6. Locally, the user assigns a unique identifier to their shipping in the form of a number. This process is once again mediated by a ZKP asserting that the generated number is both, unique and (pseudo)random. The end result of this step is a private zkLocus geolocation proof, which contains the "encrypted" metadata of the user's name and shipping address, as well as the unique identifier of the shipping.
7. The user sends the zkLocus proof and the unique identifier generated in step 6 to the e-commerce platform. The e-commerce platform has access to the generic geographical area of where the user is located, the unique identifier of the shipping, but not the user's name and shipping address.
8. The e-commerce platform ships the package to a processing center, which is located in the geographical area defined in the zkLocus proof. Alongside the package, the zkLocus proof and the unique identifier from step 7 are also sent. This processing center is independent from the e-commerce platform, such as a postal service or a courier company. Once in pocession of hte package, the processing center does not have access neither to the user's name, nor their shipping address. They only know that the user is located somewhere within the geographical area defined in the zkLocus proof.
9. In order to retrieve the shipping information, the processing center would publicly notify that they have received a package with a specific unique identifier.
10. The user awaits for the notification from the processing center with their unique identifier, and then sends the salt to the processing center. The processing center uses the salt to extract the user's name and shipping address from the metadata of the zkLocus proof. At this point, the processing center has access to the user's name and shipping address.
11. The processing center ships the package to the user's shipping address, following the standard procedures for package delivery.

In this process the user's name and shipping address are kept private from the e-commerce platform throughout the whole process. The e-commerce platform only has access to the generic geographical area of the user. Given that the processing center is independent from the e-commerce platform, the user's name and shipping address are only revealed to the processing center, once the user provides the salt to them. As such, the e-commerce platform has no way of knowing which pacakges are devliered to which users. It's also worth noting that the zkLocus operations described above are overly verbose for clarity. In reality, it's possible to compress and optimize all of the proof generation steps into a single computation. I invite you to explore the [zkLocus GitHub repository](https://github.com/iluxonchik/zkLocus) for documentation and examples on how to implement this process in practice.

Although the implemention explored here relies on the fact that the processing center and the e-commerce platform are independent entities, it's possible to extend this process to work in a zero-trust environment. One way of doing this would be to employ MPC techniques of secret sharing, to implement a [Rate-Limiting Nullifier (RNL) with collateral slashing](https://rate-limiting-nullifier.github.io/rln-docs/rln_in_details.html). This would mean that in case of data sharing/collusion of the processing center and the e-commerce platform, either party would be able to retrieve a pre-locked collateral/stake.

The entire solution as described here can be natively implemented on the [Mina protocol blockchain](https://minaprotocol.com/), by relying on the [zkLocus smart contracts](https://github.com/iluxonchik/zkLocus) and pseudo-random number generation with freshness guarantees enabled by [RandoMina](https://github.com/iluxonchik/randomina). However, it's also possible to implement this solution in any off-chain environment, such as traditional Web 2 applications and legacy systems. This is possible thanks to the [recursive zkSNARKs architecture of zkLocus](https://illya.sh/blog/posts/zklocus-authenticated-geolocation-blockchain-zk/), which allows it to operate in any computational environment. For a deeper technical dive, you can consult the [zkLocus Whitepaper](https://zklocus.dev/whitepaper), as well as the [zkSafeZones Whitepaper](https://zklocus.dev/zkSafeZones/).

### 24/7 Privacy-Preserving Global Surveillance With ZK

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/zk-private-global-surveillance.jpeg" alt="A vibrant, retro-futuristic cityscape under surveillance with drones and cameras, representing a privacy-preserving global surveillance system. The scene uses bright colors and clear details to highlight how Zero-Knowledge Proofs can maintain individual privacy even in a constantly monitored environment." style="max-width:100%; max-height:auto;border:none;">
</div>

Now that we have explored on how we can integrate privacy into ubiquitous services like search engines, social networks and e-commerce platforms, let's think bigger and explore how we can have global 24/7 hour video surveillance without compromising the privacy of individuals and groups. The idea of privacy in a state of constant survaillance may seem like an oxyomoron, and in most cases it indeed is. Unless of course... you've guessed it - we employ Zero-Knowledge Proofs.

For this example, let's focus on video surveillance. Let's say tomorrow a global video survaillance network is deployed. There are no covert areas - every corner, every alley is monitored 24/7. Even the insides of people's houses are partly monitored. We can imagine how many people would not be comfortable with such a state of survaillance, due to the privacy implications. However, we can also imagine how such a system could be beneficial for law enforcement, disaster response, and other humanitarian and security applications.

Now, let's say that the entity that installed this global video surveillance network - like the government - pretty promises you that the video cameras are used strictly within the legal framework, and the data is neither stored, nor shared with third-parties, unless a subset of law-infringement activities are detected. The cameras would be running 24/7, but they would only store and report when certain activities are detected, such an assault, an accident, a medical emergency or when facial recognition identifies individuals who are being sought by law enforcement. Well, most likely you still wouldn't be comfortable with such a soution, as there is no way to verify that the cameras are indeed used as promised. You would have to **trust** a third-party (or multiple third-parties) to keep their promise.

Okay, what if the source code of the software operating on cameras is open-sourced and easily accessible somewhere like GitHub? Well, there is still no guarantee that the code that is being run on the cameras is the same as the one that is stored on GitHub, nor that it hasn't been tampered with. Once again, you have **trust** third-parties to fulfill their promises.

It's important to note that the break of **trust** can happen for many reasons, including accidents. It doesn't necessarily need to be malicious. From the point of view of the public, the end result is the same - privacy compromise. As such, in order to design a global privacy-preserving video surveillance network, we need to remove the need to **trust** third-parties. We can do it by moving the **trust** to an entity that cannot break that trust for neither malicious, nor accidental reasons. Such an entity would be impartial, and its existance would be defined by a strict set of rules. This entity is mathematics, and there is no need to trust it in a traditional sense - if we trust that `2+2=4`, then we trust it by extension.

As such, an acceptable solution would be one where we could mathematically attest that a specific set of code is being executed on the cameras, and that this code complies with the claimed functionality. [Zero-Knowledge Proof protocols like zkSNARKs and zkSTARKs allows us to do exactly that](https://illya.sh/blog/posts/zksnark-zkstark-verifiable-computation-model-blockchain/), by enabling a verifiable computation model based on a crytographic observation of a computation. Such an approach would allow anyone to verify the code is being executed on the cameras. If this code indeed doesn't store any data unless a specific set of events or activities are dected, then the privacy is preserved. In practice, here is how such a system could operate:

1. The cameras continuously feed the live video feed into a zkML model, which labels the data in the feed as "suspicoius" or "non-suspicous".
2. The footage is only stored and reported if the zkML model labels it as "suspicious", otherwise it's discrded.
3. The used zkML model is public, allowing anyone to pass arbitrary video footage to it, and verify its output. If the model has many false positives, then this potential privacy breach would be known to the public, leading to the model being updated or replaced.

In this case, we are not leveraging the data abstraction feature of ZKP, but rather its veriable computational model. The former is a subset of the latter, as we create this data abstraction through verifiable computation. Developing such systems is easier than you may think, specially with recent developments in the area of zkVM (Zero-Knowledge Virtual Machine), which allows you to write "traditional code", whose execution is cryptographically observed by ZKP protocols like zkSNARKs and zkSTARKs. Here "traditional code", means code that follows conventional programming paradigms, such as imperative or functional, with explicit instructions provided to the computer. This is in contrast to the constraint-based programming model typically employed in ZK.

## Conclusion

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/privacy-with-zero-knowlege-proofs-zklocus/privacy-with-zero-knowledge-proofs-conclusion.jpeg" alt="A serene, retro-futuristic sunset scene depicting a calm beach with neon lights, symbolizing the conclusion of the article. The image conveys a sense of relaxation and privacy, emphasizing the overall theme of achieving digital privacy through Zero-Knowledge Proofs." style="max-width:100%; max-height:auto;border:none;">
</div>

Throughout this article we have explored how digital privacy is unfeasable, unless we rely on [verifiable computation constructs like Zero-Knowlege Proofs](https://illya.sh/blog/posts/zksnark-zkstark-verifiable-computation-model-blockchain/). Achieving privacy in the digital ralm, means shifring paradigm from protecting sensitive data that's already been shared to sharing data in an inherently private manner. The verifiable computation model of ZKPs allows us to do exacly that, by turning data into computation and abstracting it in a manner that inherently preserves its privacy.

Such an approach removes the need for us to **trust** third parties to preserve our privacy, and replaces that trust with mathematical guarantees on which ZK constructs are based. This enables us to retain full custody and control over our data while preserving the benefits and convenience of digital services.

Solutions built atop of ZKPs, like zkLocus, zkSafeZones, zkML and zkVM, as well as related constructs, such as Merkle Trees, MPC and homomorphic encryption can be used to make our digital interaction private without compromising the functionality. This allows us to build privacy-preserving search engines, social networks, e-commerce platforms, and even 24/7 global video surveillance networks, while respecting the privacy of individuals and groups.

Privacy is a fundamental human right, and this right extends to the digital realm. As a Web3 community of enterpreneurs, engineers, users and enthusiasts it's our responsibility to push for the adoption of privacy-preserving technologies, and to treat privacy as a default, not as an afterthought. We are on a path of building a better, more inclusive and equitable society at a global scale. This one of our core mission with initiatives [zkLocus](https://zklocus.dev) and [zkSafeZones](https://zklocus.dev/zkSafeZones/). If you are interested in contributing to this mission, or if you have any questions, feel free to reach out to me via [hello@illya.sh](mailto:hello@illya.sh), on [Telegram](https://t.me/illya_gerasymchuk) or [Twitter/X](https://twitter.com/illyaGera).

### Discuss 🗣️

Currently, the blog does not natively support comments. In the meanwhile, you can discuss this blog post on:

- [Twitter/X - 🚀 Privacy Is A Myth. Unless You're Using Zero-Knowledge Proofs 🚀](https://twitter.com/illyaGera/)
- Reach out to me on Telegram at [t.me/illya_gerasymchuk](https://t.me/illya_gerasymchuk), or e-mail me at [hello@illya.sh](mailto:hello@illya.sh)
