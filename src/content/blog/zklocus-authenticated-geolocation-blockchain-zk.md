---
author: Illya Gerasymchuk
pubDatetime: 2023-12-01T06:01:00Z
title: "zkLocus: Authenticated Private Geolocation Off & On-Chain"
postSlug: zklocus-authenticated-geolocation-blockchain-zk
featured: true
draft: false
tags:
  - zkLocus
  - web3
  - blockchain
  - zero-knowledge
description: "This blog post introduces zkLocus, a pioneering Zero-Knowledge, on-chain, cross-chain, and off-chain application, offering a disruptive paradigm in private and verifiable geolocation sharing. It allows users to authenticate their presence in specific geographical areas without revealing exact coordinates. zkLocus brings geolocation data onto the blockchain. It offers native bridging, native rollup functionality, and infinite proof compression. zkLocus is built on recursive zkSNARKs, making it inherently open to customization and extension."
---

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-header.png" alt="zkLocus header image" style="max-width:100%; max-height:auto;border:none;">
</div>

This blog post introduces [zkLocus](https://zklocus.dev), a pioneering Zero-Knowledge, on-chain, cross-chain, and off-chain application, offering a disruptive paradigm in private and verifiable geolocation sharing. It allows users to authenticate their presence in specific geographical areas without revealing exact coordinates. zkLocus brings geolocation data onto the blockchain. It offers native bridging, native rollup functionality, and infinite proof compression. zkLocus is built on recursive zkSNARKs, making it inherently open to customization and extension.

You can find more about zkLocus at the following links:

- [zklocus.dev](https://zklocus.dev) - The official website of zkLocus. Here you can find the latest updates about zkLocus and contact information.
- [GitHub/zkLocus](https://github.com/iluxonchik/zklocus) - The GitHub repository of zkLocus. Here you can find the source code of zkLocus.
- [x.com/zkLocus](https://twitter.com/zklocus) - The Twitter/X account of zkLocus. Here you can find the latest updates about zkLocus.
- [contact@zklocus.dev](mailto:contact@zklocus.dev) - The email address of zkLocus. Here you can contact the zkLocus team.

## Geolocation Sharing Is Needed, But It's Broken

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-geolocation-coordinate-sharing.png" alt="zkLocus - Private Geolocation Sharing" style="max-width:100%; max-height:auto;border:none;">
</div>

Geolocation data is a fundamental component of modern life. It is utilized in a myriad of applications, from navigation and logistics to social media and dating apps. However, the widespread use of geolocation data raises significant privacy concerns. The ability to track a user's location in real-time can be exploited for nefarious purposes, such as stalking or surveillance. Moreover, the aggregation of location data can be used to infer sensitive information about a user, and then use that information to manipulate the user, for instance, by targeting them with tailored advertisements.

At the same time, sharing of geolocation with third-parties is desirable, as empowers our interaction with
the digital world. Sharing your location with an e-commerce retailer like Amazon allows them to provide you with a more personalized experience, such as offering products that are available in your area. Similarly, sharing your location with a ride-sharing app like Uber enables you to quickly and conveniently hail a ride.

Currently, there is no solution to this problem. A third-party may claim that they don't store your location
data, but there is nothing enforcing that. The third-party may either accidentally or deliberately store your location data, and then use it for their own purposes. As such, a user sharing their location with a
third-party is implicitly trusting that third-party that they will not misuse their data.

## The Impossible Security Model

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-security-model.png" alt="zkLocus - Recursive zkSNARKs security model" style="max-width:100%; max-height:auto;border:none;">
</div>

Any private geolocation sharing solutions that currently exist are either overly complex, expensive, and impractical, or faulty in their security model, in the sense that they provide for semi-private location sharing, and fail to adehere to the privacy promises.

In order to achieve the promises of private location sharing, it is necessary to adehere to a zero-trust security
model. In this model, any data that leaves the user's device is considered to be public. This means that the user's location data is not shared with any third-party, and the user retains complete control over their data. The third-party cannot violate that agreement, because they do not have access to the data in the first place. This is the only way to ensure that the user's location data is not misused.

## The Problem Of Spoofable Geolocation

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-authenticated-geolocation-without-spoofing.png" alt="zkLocus offers authenticated geolocation without spoofing" style="max-width:100%; max-height:auto;border:none;">
</div>

In the current technological ecosystem, when receiving the geolocation data of third parties, we are implicitly trusting that the data is accurate. This is because there is no way to verify the authenticity of the data. For example, when a user shares their location with a ride-sharing app, the app has no way of verifying that the user is actually at the location they claim to be. The user could be spoofing their location, and the app would have no way of knowing. This is a fundamental problem, as it means that the app cannot make any decisions based on the location data, as it cannot be sure that the data is accurate. For example, the app cannot send a driver to pick up the user, as it cannot be sure that the user is actually at the location they claim to be. This is a problem that is inherent to the current technological ecosystem, and cannot be solved without a fundamental change in the way that geolocation data is handled.

The majority of exiting applications either trust the data entered by the user in an HTTP request (location in the web browser can be trivially spoofed), or utilize the IP address of the user (which can be spoofed with a VPN or a proxy).

## Geolocation With A Cryptographically Attached Timestamp

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-authenticated-timestamp.png" alt="zkLocus - Authenticated Timestamp For Location Sharing" style="max-width:100%; max-height:auto;border:none;">
</div>

All of the current solutions, besides lacking the ability to attest to the authenticity of the shared geolocation data, also lack the ability to verifiably attest to the time at which the geolocation data point was taken. This poses a fundamental problem in the context of smart contracts on the blockchain, as they essentially require real-time interaction with geolocation data updates. This, among other things, creates a need for constant access to the internet. For example, assume that a smart contract requires you to be in a certain geographical area
at a certain time. If you are not present in a certain area by a certain time, then you are penalized. This is a common use-case in supply chain management, where goods need to be delivered to a certain location by a certain time. Imagine that you were at a certain geolocation at a certain time, but you did not have access to the internet. You would not be able to prove that you were at that geolocation at that time, and you would be penalized not because you didn't comply with the contract's terms, but because you did not have access to the internet.

## Geolocation On The Blockchain

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-geolocation-blockchain.png" alt="zkLocus - Geolocation On Blockchain. Works on any EVM-chain, Ethereum, Cardano, Mina, and any other blockchain" style="max-width:100%; max-height:auto;border:none;">
</div>

If the solution to the problem of private, verifiable, and authenticated timestamped location were to be implemented on the blockchain, it would require a complex, expensive and
purpose-specific solution. This is because the blockchain is a public ledger, and as such, it is not possible to store private data on it. As such, the solution would require a complex system of oracles, which would be able to attest to the authenticity of the geolocation data, and the time at which it was taken. This would be a complex, expensive and purpose-specific solution, which would be difficult to implement, and would require a lot of maintenance. Additionally, it would provide obscure security and authenticity guarantees.

Ideally, verifying the authenticity of the geographical coordinates and the time at which they were taken should
be a trivial, decentralized, and lightweight process. It should require no more than a few lines of code. With this, the smart contract's logic can be focused on the use-case and the value that the it aims to provide,
while not compromising on the security and authenticity of the geolocation data.

## The Need For A Solution

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-geolocation-solution.png" alt="zkLocus - Geolocation Solution On Blockchain and Off-Blockchain. Works online and offline." style="max-width:100%; max-height:auto;border:none;">
</div>

As such, there is a clear need for a solution that satisfies the following requirements:

1. **Truly Private Geolocation Sharing** - The user's location data is not shared with any third-party, and the user retains complete control over their data. The third-party cannot violate that agreement, because they do not have access to the data in the first place.
2. **Verifiable Geolocation Data** - It must be possible to attest to the authenticity of the geolocation data.
   The geolocation data implicitly contains a cyrptographic authentication of all of the sources that have
   contributed to the geolocation coordinates in question. This means that the geolocation data is verifiable, and cannot be spoofed.
3. **Authenticated Timestamp** - It must be possible to attest to the time at which the geolocation data was taken. This is a fundamental requirement for smart contracts on the blockchain, and the usage of that information
   for legal purposes.
4. **Cross-Chain Compatibility** - The solution must be compatible with all blockchains, including Mina, Ethereum and Cardano. This is a fundamental requirement for the solution to be widely adopted, and to be able to be used in a variety of use-cases and applications.
5. **Off-Chain & Legacy Compatibility** - The solution must be compatible with off-chain applications, such as web browsers, mobile devices, and IoT devices. This is a fundamental requirement for the solution to be widely adopted, and to be able to be used in a variety of use-cases and applications. Most of the existing infrastructure is off-chain, and a significant part of it is dependent on legacy systems, which cannot be easily migrated to the blockchain. As such, the solution must be able to be integrated into existing infrastructure, and must be able to be used in a variety of use-cases and applications.
6. **Cheap & Easy To Use** - The solution must be cheap and easy to use. This is a fundamental requirement for the solution to be widely adopted, and to be able to be used in a variety of use-cases and applications. The solution must be able to be easily integrated into any existing infrastructure with ease. In software engineering
   terms, this means that the solution must have a simple and intuitive API, and must be easy to integrate into any existing infrastructure with a few lines of code. Morever, integrating the solution must not require a
   significant cost in terms of time, resources and money.
7. **Flexible & Customizable** - The solution must be flexible and customizable. A business must be able to
   easily tailor the solution to their specific needs, the legislation, and internal policies.
8. **Works On Mobile** - The solution must operate on mobile devices, such as iPhones and Androids. This is fundamental, mobile phones are the most widely used computing devices, in both business and personal contexts. As such, the solution must be able to be used on mobile devices, and must be able to be integrated into any existing infrastructure with ease.

## zkLocus Is The Solution To All Problems

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-the-solution-to-geolocation.png" alt="zkLocus is the solution to all geolocation sharing and on-chain bridging challenges" style="max-width:100%; max-height:auto;border:none;">
</div>

zkLocus is the solution to all of the problems explored throughout this blog post. zkLocus is a pioneering Zero-Knowledge, off and on-chain application and application framework, offering a disruptive paradigm in private and verifiable geolocation sharing. It allows users to authenticate their presence in specific geographical areas, at specific timestamp interval, without revealing exact coordinates or timestamp. For example, a user can prove they are within the European Union, in the month of January 2024, without disclosing their precise location or its timestamp. Additionally, zkLocus provides the flexibility for users to share precise coordinates and timestamps when desired, and supports for semi-private sharing with selected group of entities.

# How zkLocus Disrupts Geolocation In Web3

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-web3-geolocation.png" alt="zkLocus is private and decentralized geolocation for Web3" style="max-width:100%; max-height:auto;border:none;">
</div>

zkLocus is a pioneering Zero-Knowledge, cross-chain, and off-chain application, offering a disruptive paradigm in private and verifiable geolocation sharing. It allows users to authenticate their presence in specific geographical areas without revealing exact coordinates. For example, a user can prove they are within the European Union without disclosing their precise location. Additionally, zkLocus provides the flexibility for users to share precise coordinates when desired, with the option for semi-private sharing with selected entities.

### Customizable and Extensible recursive zkSNARKs

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-recursive-zksnarks-architecture.png" alt="zkLocus is architected on recursive zkSNARKs" style="max-width:100%; max-height:auto;border:none;">
</div>

Functioning as both an application and a framework, zkLocus is built on recursive zkSNARKs, making it inherently open to customization and extension. This architecture enables users to easily introduce custom geolocation sources. For instance, a business could integrate its own location tracking system into zkLocus for enhanced logistics management. The platform also supports arbitrary geographical assertions, offering diverse applications in various domains.

### Bringing Geolocation On-Chain

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-bringing-geolocation-on-chain.png" alt="zkLocus is providing real-world geographical coordinates on-chain. zkLocus is cross-chain compatible, and works on any EVM chain and the Mina Blockchain" style="max-width:100%; max-height:auto;border:none;">
</div>

A key aspect of zkLocus is its ability to bring geolocation data onto the blockchain in a decentralized, verifiable manner. zkLocus offers native bridging. While it operates natively on the Mina Blockchain, its compatibility extends to Ethereum, Cardano, and any other smart-contract blockchain. This cross-chain functionality allows for the independent, on-chain verification of ZK proofs generated by zkLocus. For example, a supply chain application on Ethereum can use zkLocus to verify the location of goods without relying on a centralized system. This data can then be utilized in the associated smart contract logic in arbitrary ways. If a set of goods fails to be in a certain geographical area by a certain time, then the responsible counterparty pays a penalty. Individual good tracking is also possible. For example, when purchasing goods from a company, it is possible to verify that those goods have never transacted with a sanctioned nation, thus making this trade compliant with the law. Throughout all of this process, the exact location or the path taken by the good is never shared. This allows each business to utilize any data sharing and storing policy that they desire. The policy can be trivially integrated into the existing infrastructure, the company’s compliance policy, and the law.

### Native Rollup With Infinite Proof Compression

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-native-rollup.png" alt="zkLocus offers native roll-up, thanks to its architecture which relies on recursive zkSNARKs" style="max-width:100%; max-height:auto;border:none;">
</div>

zkLocus offers native roll-up functionality. This means that zkLocus proofs can be used with other zkLocus proofs, and combine multiple proofs together. In this sense zkLocus is its own “Layer 2”. Moreover, zkLocus proofs can be submitted on-chain by any entity. This enables for a mesh integration, and an infinite set of decentralized “mempools”. This is enabled by the application's infinite proof compression capability, thanks to its recursive zkSNARK architecture. Users can condense extensive location records into a single proof. For instance, a user could compile a week's worth of location data into one proof for verification, choosing to share either detailed coordinates or just a general area presence. For instance, consider a user who tracks their location every second on their iPhone over the course of a week, accumulating a total of 604,800 individual location records. Utilizing zkLocus, they can consolidate all these discrete proofs into a single, comprehensive proof. This consolidated proof encapsulates a week’s worth of data and can be transmitted to any third-party for verification. The user retains complete control over the granularity of the information shared. They have the option to disclose the exact coordinates and timestamps for each of the 604,800 records, offering a detailed chronicle of their movements. Alternatively, the user can opt for a more generalized proof, simply verifying their presence within a broader geographical region. For example, they might choose to validate that throughout the entire week, they remained within the borders of Germany, while deliberately omitting specific details about their precise locations within the country. This flexibility allows for tailored privacy levels, catering to the user's needs for either detailed disclosure or broader area verification. A “user” here can be a human, an application running on the iPhone, an automated drone, a Raspberry Pi inside of a truck transporting goods, or any other programmable entity.

### zkLocus Use-Cases In DeFi

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-defi-decentralized-finance.png" alt="zkLocus in Decentralized Finance (DeFi) applications and use cases" style="max-width:100%; max-height:auto;border:none;">
</div>

Bringing geolcoation on-chain with zkLocus enables a myriad of use-cases in the DeFi space. It allows for the creation of financial derivatives, whose value is tied to geolocation For example, an ERC-20 that charges a different fee or tax, based on the location where it was utilised. This enables a currency that is taxed more outside of the country that has created that currency, creating incentives for it to transacted with domestic businesses.

### Enables Digital-Era Laws

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-digital-law.png" alt="zkLocus in digital law, enforceable by smart contracts, and generation of legal assertions regarind geoloation" style="max-width:100%; max-height:auto;border:none;">
</div>

The applications of zkLocus extend beyond blockchain and the mere sharing of verified locations. zkLocus pioneers the construction of digital-era legal frameworks, leveraging its decentralized, transparent, portable, and tamper-proof nature. These attributes make zkLocus proofs exceptionally suited for legal applications. An example of such application is the integration of zkLocus with AI models and automated systems, such as a drone purchased in Germany. Incorporating zkLocus into the drone's software can restrict its operational and recording capabilities to specified geographic zones. This enables enforceable digital laws applicable to autonomous entities, including A.I. systems like Large Language Models. Such an approach not only enhances user privacy but also ensures adherence to legal statutes without infringing on the liberties of citizens. For instance, should a dispute arise where a user is accused of recording in a prohibited area, zkLocus provides a robust defense. The user can trivially refute such claims by presenting the zkLocus location proof, intrinsically linked to the drone’s media. Owing to the nature of zkLocus’s zero-knowledge proofs, they find versatile applications across various platforms, from blockchain and traditional Web2 environments to IoT devices and more, bridging the gap between technology and legal compliance.

### Runs On Mobile and Web. JavaScript API. Easy Integration.

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-mobile-support-javascript.png" alt="zkLocus offers an easy and intutive JavaScript API and TypeScript API." style="max-width:100%; max-height:auto;border:none;">
</div>

zkLocus has an intuitive JavaScript API, and it can be installed directly from npm. This means zkLocus runs anywhere where JavaScript runs. This means that zkLocus’ has native web browser compatibly, including on mobile devices like iPhones and Androids. This enables an easy and flexible integration into existing the web infrastructure. A practical application is for websites needing to comply with GDPR; a user can prove they are outside the EU, negating the need for cookie consent dialogs without revealing their actual location. The proof does not expose the user’s exact location, thus not compromizing their privacy. At the same time, the business has the removed complexity of storing the data and complying with GDPR. In fact, such an application for the cookie policy would likely not even be even subject to GDPR, as the information stored is very broad.

### Steps To Integrate zkLocus In A Business

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-business-integration.png" alt="How to integrate zkLocus into your business or application" style="max-width:100%; max-height:auto;border:none;">
</div>

From a practical perspective, here is how zkLocus can be integrated into any business for a variety of use-cases with extreme ease:

1. The business defines a custom GeoLocation Oracle. This could be something as simple as exposing a RESTful HTTP endpoint which returns the coordinates for the request. The code determining the coordinates of the request can be implemented in any programming language, and utilize arbitrary logic. For example, it could be a web service implemented in Java, which internally performs API requests, and consults a MySQL database in order to provide an answer.
2. A Zero-Knowledge circuit that reads data from that Oracle and returns the geographical coordinates is implemented. This takes a couple of lines of code.
3. An entry point of geographical coordinates from the custom oracle source is defined in zkLocus.

In merely three steps, a business has successfully implemented a custom geolocation source and has enabled zkLocus to support proofs derived from this source. Consequently, the business is now capable of generating verified geolocation zero-knowledge proofs, which they can utilize for a variety of purposes. These applications include storing the proofs in a MongoDB database for legal compliance, bridging them onto the Ethereum blockchain for integration in their smart contracts, or directly incorporating them into their AI models to verify the execution of certain models within approved geolocations.
Furthermore, in terms of time efficiency, this comprehensive three-step process can be completed in approximately one hour.

## ✨ zkLocus: Authenticated Geolocation For Web 3 ✨

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/zklocus/zklocus-geolocation-for-web3.png" alt="zkLocus is authenticated geolocation for Web3" style="max-width:100%; max-height:auto;border:none;">
</div>

This blog post introduced zkLocus, communicated its value proposition, and presented its vision. zkLocus is a pioneering Zero-Knowledge, on-chain, cross-chain, and off-chain application, offering a disruptive paradigm in private and verifiable geolocation sharing. zkLocus brings geolocation data onto the blockchain. It offers native bridging, native rollup functionality, and infinite proof compression. zkLocus is built on recursive zkSNARKs, making it inherently open to customization and extension.

For more information about zkLocus, visit one of the following links:

- [zklocus.dev](https://zklocus.dev) - The official website of zkLocus. Here you can find the latest updates about zkLocus and contact information.
- [GitHub/zkLocus](https://github.com/iluxonchik/zklocus) - The GitHub repository of zkLocus. Here you can find the source code of zkLocus.
- [x.com/zkLocus](https://twitter.com/zklocus) - The Twitter/X account of zkLocus. Here you can find the latest updates about zkLocus.
- [contact@zklocus.dev](mailto:contact@zklocus.dev) - The email address of zkLocus. Here you can contact the zkLocus team.
