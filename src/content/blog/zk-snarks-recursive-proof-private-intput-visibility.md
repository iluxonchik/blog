---
author: Illya Gerasymchuk
pubDatetime: 2023-12-03T16:00:00Z
title: Recursive zkSNARK Proof as a Private Input - What Is Visible To The Verifier?
postSlug: zk-snarks-recursive-proof-private-intput-visibility
featured: false
draft: false
tags:
  - dev
  - o1js
  - zkLocus
  - zero-knowledge
description: "In this blog post, we will explore the dynamic intersection of blockchain, privacy, and cryptography, focusing on the pivotal role of zero-knowledge proofs, especially zkSNARKs and their recursive variations. These innovative technologies are revolutionizing our approach to digital privacy and authenticity. Here, we delve into their practical applications, discussing their significance in Zero-Knowledge circuits, the Mina blockchain, and the O1JS framework. Additionally, we will examine how recursive zkSNARKs fuel applications like zkLocus, enabling users to authenticate and share their location while preserving privacy. By the end of this post, you'll have a comprehensive understanding of Zero-Knowledge applications, Zero-Knowledge ciructis, zkSNARKs, recursive zkSNARKs, and what exactly is visible to the verifier when a recursive proof is passed as a private input in a zkSNARKs circuit."
---

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-1.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

In the fascinating world of blockchain, privacy and cryptography, zero-knowledge proofs have emerged as a pivotal concept, revolutionizing the way we think about privacy, authenticity and decentralization. These proofs enable one party to prove to another that a statement is true without revealing any information beyond the validity of the statement itself. But how do these principles translate into practical applications, especially in the realm of zkSNARKs (Zero-Knowledge Succinct Non-Interactive Argument of Knowledge)? And more intriguingly, what happens when we dive into the realm of recursive zkSNARKs? In this blog post, we will unravel the complexities of zkSNARKs and their recursive counterparts, examining their role and implications in Zero-Knowledge applications, Zero-Knowledge circuits, the Mina blockchain and the O1JS framework. We will analyze how recursive zkSNARKs are implemented under the hood, and what exactly is visible to the verifier when a recursive proof is passed as a private input in a zkSNARKs circuit. We'll not only delve into the theoretical underpinnings of these concepts but also put them into practice. We will walk through the process of writing a recursive zkSNARK program in O1JS, providing a tangible demonstration of these principles at work. Moreover, we will explore how recursive zkSNARKs are used in [zkLocus](https://zklocus.dev/), a Zero-Knowledge application that enables users to authenticate and share their geographical location while preserving their privacy. By the end of this post, you'll have a comprehensive understanding of the nuances of recursive zkSNARKs and their practical applications, well-equipped to apply these concepts in your own blockchain, privacy, and cryptography endeavors.

### Unveiling the Mysteries of Recursive zk-SNARK Proofs: A Deep Dive into Visibility and Privacy

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-2.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

Have you ever wondered about the extent of visibility a verifier has in a zkSNARKs circuit, especially when a recursive proof is used as a private input? More specifically, about the following questions:

- What does the verifier see?
- Can the verifier attest which code (zero-knowledge circuit) was executed to produce the proof?
- Can the verifier see the public output of the recursive proof?
- Can the verifier see the public input of the recursive proof?
- Can a circuit receiving a recursive proof as input access all the data used in the inner proof?

Understanding recursive zkSNARKs means understading and correctly answering these questios. This knowledge forms the bedrock of understanding how recursive zkSNARKs proofs operate within the broader landscape of zero-knowledge proofs, influencing both the design and practical application of these systems. The goal of this blog post is to make the answer to each one of these questions very clear.

# Introduction & Motivation

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-3.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

The Zero-Knowledge application development field is unboubtably the most disruptive and groundbreaking area of work not only in sofware engineering, but in the technoglogy field as a whole. While Zero-Knowldge technonolgy has been around since the 1980's, its usage only became practical for real-world application with the advent of zkSNARKs in 2010, and it's only in the last year that the development of Zero-Knowledge circuits and applications by non-mathematicians, academics, and cryptographers has become possible. This is thanks to the development of Zero-Knowledge application frameworks, such as [O1JS](https://docs.minaprotocol.com/zkapps/o1js), which provides a clean and easy to use abstration for the creation of zero-knowneldge circuts. In this blog post, we will use O1JS to build a calculator application using recursive zkSNARKs, and analyze what exactly is visible to the verifier when a recursive proof is passed as a private input in a zkSNARKS circuit.

## The Zero-Knowledge Community

The community of developers focusing on the development of Zero-Knowledge applications is not very large, but it is very dedicated to
the cause. While there is a lot of work and interest, there is a lack of depper understanding, due to the lack of avaialable information.
Zero-Knowledge is the closet thing that we have to magic in the area of secure computing, and while that magic is open-source, it
is necessary to understand it in order to use it correctly. Given this, the sharing of knowledge is essential for the development of the field.

## Zero-Knowledge Applications: zkLocus

<div style="text-align:center; max-width:97%;">
<a href="https://zklocus.dev">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-homepage.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</a>
</div>

I am curretly developing on a Zero-Knowledge application called [zkLocus](https://zklocus.dev). zkLocus enables users to authenticate and share their geographical location while preserving their privacy. By using recursive zkSNARKs, users can verify being within a certain region, following a certain path, or being outside a specified area, without revealing their precise coordinates.

You can find more about zkLocus at [zklocus.dev](https://zklocus.dev). I have also written an essay on zkLocus, explaining its value, features, and use-cases. You can read it at [zkLocus: Authenticated Private Geolocation Off & On-Chain](/blog/posts/zklocus-authenticated-geolocation-blockchain-zk/).

As a part of an effort to contribute to the expansion of knowledge in the field, I am commiting to writing a series of blog posts on the topic of Zero-Knowledge applications. In them, I will share my learnings and insights from my work on zkLocus, and I aim to explain the concepts in a way that is easy to understand for anyone in this field. This includes
developers, researchers, entrepreneurs, and other enthusiasts.

# Zero-Knowledge Applications

Before we dive into the topic of recursive zkSNARKs, it is important to understand what exactly Zero-Knowledge
applications are. Zero-Knowledge applications differ from traditional applications. In traditional software,
like in web or mobile applications, the application delivers its functionality by executing the set of
insturctions, loops and conditions that are defined in its source code.

Zero-Knowledge applications are different. A Zero-Knowledge application does not does not deliver its end goal
by executing a set of pre-determined instructions. Instead, the set of instructions that a Zero-Knowlede
application executes, produce an assertion about the end goal.

## Zero-Knowledge Application vs Traditional Application

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-6.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

Let's explore this with an example, by demonostrating how the same application can be implemented as a traditional
application, and as a Zero-Knowledge application. Let's say that we want to implement an application that
tells you current temperature in Celsius, in the city of Dublin, Ireland.

If we were to implement this as a traditional application in the form a website that displays the temperature
in Dublin, we would focus our implementation on the following:

1. Fetching the current temperature in Dublin from an API.
2. Displaying the temperature to the user.

The code for this would look something like this:

```javascript
const temperature = await fetchTemperatureInDublin();
displayTemperature(temperature);
```

This is an imperative approach that the majority of the software engineering community will be familiar with.
The application's focus is to deliver the tempreature to the user in Dublin at the current time. As such, it focuses
on obtaining that data and returning it to the end-user. There is an implicit assumption that the information that will
be presented to the user is correct, and that all of the systems that are involved in the process delivering the end
functionality are cooperative in nature. Sure, we may be using HTTPS/TLS to ensure that the data is not tampered by
a third-party, but we are not commiting that that is the temperature that will be displayed to the user. We are assuming
that the code that will execute on the end-user's device will be the code that we have written. We also assume that at
no point someone may tamper with the data that is displayed to the user. We are assuming that the temperature that will
be displayed to the end user is infact the temperature that was returned by the API call, and that that temperature indeed
represents the **real** temperature in Dublin at the current time, with all of the sources of that claim being open and
verifiable.

But Zero-Knowledge applications are different. In fact, Zero-Knowlede applications aren't even traditional
applications, in the sense that you are not abstracting the instructions for the processor to store,
retrieve, and operate on data in physcial registers. A Zero-Knowlde application is an equation, a polynomial.
Verifying a Zero-Knowledge proof, means satisfying the variables of a very complex polynomial.

The end product of any Zero-Knowledge application is a circuit, which is a set of constraints that define a
polynomial. There are no REST API calls to fetch some data from an external endpoint, or manipulation of the DOM in in the browser to display information to the end user visually. Instead, the end product of a Zero-Knowledge
application, proof or circuit is a cryptographic assertion about a fact. As such, it is necessary to adapt the mental model when developing Zero-Knowledge applications.

In Zero-Knowledge applications, the focus is not on the end goal, but on the assertions about the end goal.
In our example, the Zero-Knowledge application would not be focused on fetching the temperature in Dublin,
but on producing a proof that attests to the fact that the temperature in Dublin the the current time is 10
degrees Celsius. By observing this proof, a third party can verify with precision the validity of the proof,
as well as the source of all of the data. In other words, a Zero-Knowledge application attesting the
temperature in Dublin at current time, would focus on the following:

1. Verifiably obtaining the current temperature in Dublin. This could mean using an Oracle, or another
   Zero-Knowledge application veriably obtains the temperature in Dublin at the current time.
2. Expose a a derivative of the fact. By derivative of the fact, I mean any information that derives from that
   fact, and allows a third-party (the verifier) to extract useful information from that derivative in a cryptographically verifiable manner. For our example, this simply means exposing the temprature value.

As such, our Zero-Knowledge application will generate a proof, that returns a single value: the current
temperature in Dublin. That value will be an integer (more precisely a _field_), and it will be the public
output of our Zero-Knowledge application/circuit. Even though our ZK application only has a single output - the numeric value of current temperature in Dublin, that output comes bundled with assertions about output, namely:

1. The source of the temperature. This means that the proof demonstrates exactly where the temperature value
   has been obtained from. Was it obtained by an HTTP request to an Oracle? Was it read direly from a hardware sensor?
2. A guarantee that the obtained value refers to the temperature in Dublin at the current time. This means that
   the proof embeds assertions not only about the freshness of the temperature value, but also that that value
   is tied to a particular instant in time. The source of that time instant, as well as its correlation to the
   temperature reading must also be cryptographically asserted in that proof.

Given this, the code of our Zero-Knowledge application would look something like this:

```javascript
const temperatureValueProof = await generateTemperatureProof();
const temperatureLocationProof = await generateTemperatureLocationProof();
const temperatureTimeProof = await generateTemperatureTimeProof();

const temperatureWithLocationProof = await attachLocationToTemperature(
  temperatureValueProof,
  temperatureLocationProof
);

// proof that the ZK Application will return
const temperatureWithLocationAndTimeProof =
  await attachTimeToTemperatureAndLocatio(
    temperatureWithLocationProof,
    temperatureTimeProof
  );
```

In this way, a Zero-Knowledge application is a set of assertions about a fact. The fact is the temperature
in Dublin at the current time, and the assertions ensure that the source of the temperature, the location of the
temperature, and the time of the temperature are the claimed ones. The Zero-Knowledge application is not focused on delivering
the temperature to the end-user, but on delivering the assertions about the temperature to the end-user.

The end product of a Zero-Knowledge application is a proof. That proof represents **an observation of a computation**.
That proof is the result of observing some computation, and attesting that the computation was performed correctly, and
exactly as described by its source code. By looking at a Zero-Knowledge proof, a third-party can verify exactly which
code was executed to produce the proof, and what was the result of that execution. If you design a Zero-Knowledge application
which adds two numbers, the resulting proof of that computation will attest to the fact that two numbers were provided as input
and their sum was the output. If you design a Zero-Knowledge application that fetches the temperature in Dublin, the resulting
proof will attest to the fact that the temperature was fetched from a particular source, and that the temperature is the one
that was claimed.

Having this common ground of understanding, we are ready dive into the techncial details of zkSNARKs and recursive zkSNARKs.

# Recursive zkSNARKs Explained

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-7.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

In order to understand what recursive zkSNARKs are, we first need to understand what zkSNARKs are. But before we explain what
zkSNARKs are and how they work, let's first explain what relation zkSNARKs have to Zero-Knowledge circuits.

## Zero-Knowledge Circuits and zkSNARKs

A zero-knowledge circuit is a necessary but not sufficient component for creating a zero-knowledge proof. It defines and structures the problem but doesn't inherently solve it. The actual proving mechanism – the dynamic process of generating and verifying a proof that fits within this structure without revealing any secret information – is provided by protocols like zkSNARKs. The circuit is the framework; zkSNARKs are the tools and methods used to construct and verify the proof within that framework.

### Zero-Knowledge Circuits

A zero-knowledge circuit is a way to represent a computational problem or a logical statement in a structured format. It's similar to how a mathematical equation represents a problem in algebra. The circuit breaks down the problem into basic operations or steps. These operations are usually quite simple and fundamental, such as addition, multiplication, or logical comparisons. The operations in the circuit are organized in a sequence or a network, where the output of one operation can serve as an input for another. This sequential structure mimics the logical steps one would take to solve a problem or verify a statement.

The circuit can be thought of as a blueprint or a map. Just as a blueprint for a building outlines the structure without actually being the building itself, a zero-knowledge circuit outlines the logical structure of a proof without being the proof.
The circuit essentially describes what needs to be proven – the 'problem statement' in a computational form. It doesn’t, however, execute the proof. It just lays out the framework within which the proof needs to fit.

A zero-knowledge circuit, by itself, is static. It doesn't change or adapt; it's just a representation. While the circuit defines the problem, it does not inherently provide a method for proving that the problem can be solved (or a statement is true) without revealing the secret information. It's akin to having a question without the process for answering it.

### zkSNARKs

While a zero-knowledge circuit can represent a problem, zkSNARKs provide the machinery to actually generate a cryptographic proof based on that representation. zkSNARKs use complex mathematical algorithms to produce a proof that the statements represented in the circuit are true, without revealing any of the underlying data.

zkSNARKs also include efficient verification processes. These allow a verifier to quickly and reliably check the validity of the proof without interacting with the prover or accessing any of the secret information implied in the proof.

zkSNARKs are particularly valued for their efficiency. They produce succinct proofs that are small in size and quick to verify, which is crucial for applications like blockchain, where speed and minimal data transmission are essential.

Another important aspect of zkSNARKs is their non-interactive nature. The prover can generate proofs without any further interaction with the verifier. This means that the prover can generaet a proof without being in active interaction with the verifier. You can generate a proof fully offline, and then send it to the verifier later. This is advantageous in many scenarios, especially in decentralized systems.

Now, armed with the knowledge of what zkSNARKs are, we can explain what recursive zkSNARKs are, how they work, how do they differ from zkSNARKs, and what computational paradigms they enable.

## Recursive zkSNARKs

Recursive zkSNARKs take the concept of zkSNARKs a step further. In recursive zkSNARKs, you can prove that you have a valid proof (or multiple proofs) of a statement, without actually knowing the original statement or the details of the proof itself. This is like proving that a chain of logic or a series of statements is correct, without needing to know all the details of each individual statement.

This enables a myriad of use-cases that are infesable with regular zkSNARKs. Over the remainder of this blog post, we will continue
deepening our understanding of recursive zkSNARKs, and explore how they are used in Zero-Knowledge applications. In fact, very soon
we will be writing our own Zero-Knowledge application using recursive zkSNARKs, and analyzing what exactly is visible to the verifier
when a recursive proof is passed as a private input in a zkSNARKS circuit.

# zkSNARKs and Recursive zkSNARKs Explained With an Example

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-8.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

In other words, recursive zkSNARKs is what you get when you combine zkSNARKs with magic ✨. zkSNARKs allow you to prove a knowledge of the fact to a third party while knowing that fact yourself. Recursive zkSNARKs allow you to prove a knowledge of a
fact to a third party, **without knowing that fact yourself**.

But what does that mean in practice? Let's explore this with an example.

## Recursive zkSNARKs In Practice: Supply Chain

Imagine a scenario in a supply chain where multiple parties are involved – manufacturers, transporters, distributors, and retailers. Each party needs to verify that certain conditions are met at each stage of the supply chain, but they don't need (or are not allowed) to know the specific details of the other stages. For instance, a retailer needs to ensure that a product was manufactured according to certain standards, transported under prescribed conditions, and stored appropriately by the distributor.

In a traditional setup, each party might have to reveal detailed proof of their compliance, which could include sensitive or proprietary information. With zkSNARKs, each party can prove their compliance without revealing those details.

Recursive zkSNARKs add another layer. Suppose the distributor has a zkSNARK proving that the transporter did their job correctly, and the transporter has a zkSNARK from the manufacturer. The distributor can use a recursive zkSNARK to prove to the retailer that the entire chain of custody up to that point has been handled correctly, without the distributor knowing all the details of the manufacturing or transportation processes. They just know that valid proofs exist for each step.

This allows for a chain of trust to be established where each party can be confident that all previous steps were performed correctly, without any single party having to have full knowledge of or access to all the details of every step.

## Ready, Set, NowLetsWriteSomeCode 🚀

Now, armed with the knowledge of what Zero-Knowledge applications, Zero-Knowledge circuits, zkSNARKs, and recursive zkSNARKs are, we are ready to transition into the practical part, and code and analyze a Zero-Knowledge application that uses recursive zkSNARKs. For this, we will use the O1JS framework, which is the Zero-Knowledge application framework for writing zero-knowledge proofs and smart contracts for the Mina Blockchain.

In the process, we will also answer the question of what exactly is visible to the verifier when a recursive proof is passed as a private input in a zkSNARKs circuit. But we're not going to just explain it in theory, we're going to reason about
it using the code of a real Zero-Knowledge application.

# Recursive zkSNARKs: A Powerful Tool For Zero-Knowledge Applications

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-10.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

As explained in detail above, recursive zkSNARKs are a powerful tool that enables the creation of Zero-Knowledge applications with complex
assertions, while maintaing a compact size. They allow for the creation of zkSNARK proof that receive other
zkSNARK proofs as private inputs. A zkSNARK that receives another zkSNARK as an input is able to verify that
proof, and extract its public output. This means that a proof can be used as a building block for another proof,
and that proof can be used as a building block for another proof, and so on. In this process, the full set of
knowledge and assertions provided by all of the involved proofs is compressed into a single proof. That proof
attests to the validity and the correctness of the execution of itself and all of the involved zkSNARKs which where provided recursively at any level of the proof's execution.

In essence, recursive zkSNARKs enable for the creation of Zero-Knowledge applications, which can verifiably
interact with other Zero-Knowledge applications, and attest to the validity of their execution. A ZK application
interacts with another ZK application through one of the following means:

1. A ZK application `A` receives a proof of ZK application `B` as an input, and verifies it.
2. A ZK application `A` receives a proof of ZK application `B` as an input, verifies it, and uses its public output as a part of its own computation.
3. A ZK application `A` generates the a proof for ZK application `B` as a part of its own computation, and
   interacts with that proof, by a combination `1` and/or `2`.

### Public Input and Private Input in zkSNARKs

When developing Zero-Knowledge applpictions that leverage recursive zkSNARKs, one of the first questions that
you may have is wether the recursive proofs are passed as private input, a public input, or either. In simple
terms, **recursive zkSNARKs are always passed as a private input**. However, you can always turn your
recursive zkSNARK into a public input, by exposing data in the public output of the proof that receives the
recursive zkSNARK as a private input.

It is important to note that wether the recursive zkSNARKs are passed as private or public inputs is implementation-dependent. This blog post focues on the way it is impelemented in O1JS, which is the Zero-Knowledge application framework for the Mina Blockchain. Given the convertibility of the
private input into a public input described above, the implementation details have no relevancy to the
information explored in this blog post, and the concepts explored here apply to any recursive zkSNARKs.

# Recursive zkSNARKs Calculator Application Implemented In O1JS

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-9.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

Talking about recursive zkSNARKs is great, but implementing a Zero-Knowledge application that uses recursive zkSNARKs is greater.
In this section, we will implement a Zero-Knowledge application that uses recursive zkSNARKs in O1JS. We will implement a calculator.
You may have implemented a calculator before, but have you implemented a calculator using a recursive zkSNARKs architecture? After
finishing this section, your the answer to that question will be a "yes" 🚀

### Source Code

The full source code of the recursive zkSNARKs calculator and the instructions to execute it are available at [Github.com/Recursive-zkSNARKs-Calculator](https://github.com/iluxonchik/recursive-zkSNARKs-calculator/)

### Implementing A Recursive zkSNARKs Application In O1JS

Now, as promised let's explore a practical example of a Zero-Knowledge Application using zkSNARKs. The code examples below, and all of the code examples that follow will be written in `O1JS` version `0.14.2`.

Throughout the rest of the blog post we will focus on a Zero-Knowledge application that implements
a calculator. The calculator will be limited to performed addition between two numbers. As such, our calculator
Zero-Knowledge application will be composed of two circuits:

1. A circuit that receives two numbers as public input, and outputs their sum.
2. A circuit that receives the proof of execution of the sum of two numbers as a private input, and another number as a public input, and outputs the sum of the two numbers from the first circuit, and the third number.

Implemented in `O1JS` using `TypeScript`, it will look like the following:

```typescript
/*
 * This is an implementation of a calculator using recusrive zkSNARks.
 */
import { Int64, ZkProgram, Struct } from "o1js";

export class NumbersToSum extends Struct({
  first: Int64,
  second: Int64,
}) {}

/*
 * Verifiably sum two numbers.
 */
function addBase(numbers: NumbersToSum): Int64 {
  const first: Int64 = numbers.first;
  const second: Int64 = numbers.second;

  return first.add(second);
}

/*
 * ZK circuit that verifiably sums two numbers.
 */
export const BaseCalculatorCircuit = ZkProgram({
  name: "Base Calculator Circuit",
  publicInput: NumbersToSum,
  publicOutput: Int64,

  methods: {
    add: {
      privateInputs: [],
      method: addBase,
    },
  },
});

/*
 * Proof type for the base calculator circuit. This is how we define the recursive proof type in O1JS.
 */
export class BaseCalculatorCircuitProof extends ZkProgram.Proof(
  BaseCalculatorCircuit
) {}

/*
 * Recursive zkSNARK number addition. The method receives a number as public input, and the recursive proof of sum of two numbers as a private input. The method verifies the proof, and outputs the sum of the numbers from the proof, and the number from the public input.
 */
function addRecursive(
  number: Int64,
  sumProof: BaseCalculatorCircuitProof
): Int64 {
  // 1. Verify that the proof is valid
  sumProof.verify();

  // 2. Extract the public output from the proof. The public output is the sum of of two numbers.
  const sum: Int64 = sumProof.publicOutput;

  return number.add(sum);
}

/*
 * ZK circuit that recursively sums a number to a proof of sum of other numbers.
 */
export const RecursiveCalculatorCircuit = ZkProgram({
  name: "Recursive Calculator Circuit",
  publicInput: Int64,
  publicOutput: Int64,

  methods: {
    add: {
      privateInputs: [BaseCalculatorCircuitProof],
      method: addRecursive,
    },
  },
});

/*
 * Proof type for the recursive calculator circuit. This is how we define the recursive proof type in O1JS.
 */
export class RecursiveCalculatorCircuitProof extends ZkProgram.Proof(
  RecursiveCalculatorCircuit
) {}
```

The code above defines two circuits, `BaseCalculatorCircuit` and `RecursiveCalculatorCircuit`. The first circuit, `BaseCalculatorCircuit` is a simple circuit that receives two numbers as public input, and outputs their sum. The second circuit, `RecursiveCalculatorCircuit` is a recursive circuit that receives a number as public input, and a proof of the execution of the first circuit as a private input. The second circuit verifies the proof, and outputs the sum of the number from the public input, and the sum of the two numbers from the public ouput of the proof received as a private input.

The code above also defines two proof types, `BaseCalculatorCircuitProof` and `RecursiveCalculatorCircuitProof`. These are the types of the proofs that are generated by the circuits. The `BaseCalculatorCircuitProof` is a proof of the execution of the `BaseCalculatorCircuit`, and the `RecursiveCalculatorCircuitProof` is a proof of the execution of the `RecursiveCalculatorCircuit`. These proof types will be used to verify the proofs, and extract the public output from them. This explicit definition is necessary due to the nature of Zero-Knowledge circuits: all of the data type types and sizes must be known and fixed.

### Zero-Knowledge Calculator Application: Usage

The zero-knowledge application calculator above can be used as follows:

```typescript
// 1. Compile both ZK Applications / ZK Circuits

console.log("Compiling Base Calculator ZK Applications / ZK Circuit...");
const baseCalculatorCircuitVerificationKey =
  await BaseCalculatorCircuit.compile();

console.log("Compiling Recursive Calculator ZK Applications / ZK Circuit...");
const recursiveCalculatorCircuitVerificationKey =
  await RecursiveCalculatorCircuit.compile();

// 2. Create a proof for the base sum
const numbersToSum = new NumbersToSum({
  first: Int64.from(10),
  second: Int64.from(20),
});

console.log("Creating a proof for the base sum...");
const sumProof: BaseCalculatorCircuitProof =
  await BaseCalculatorCircuit.add(numbersToSum);

// verify to ensure that the proof is valid
sumProof.verify();

const numbersInSum: NumbersToSum = sumProof.publicInput;
const resultOfSum: Int64 = sumProof.publicOutput;

console.log(
  `The result of the sum of ${numbersInSum} numbers is ${resultOfSum}`
);

// 3. Create a recursive sum proof

const numberToSum: Int64 = Int64.from(100);
const recursiveSumProof: RecursiveCalculatorCircuitProof =
  await RecursiveCalculatorCircuit.add(numberToSum, sumProof);

// verify to ensure that the proof is valid
recursiveSumProof.verify();

const resultOfRecursiveSum: Int64 = recursiveSumProof.publicOutput;

console.log(`The result of the recursive sum is ${resultOfRecursiveSum}`);
```

#### Execution Output

Executing the code above will produce the following output:

```
Compiling Base Calculator ZK Applications / ZK Circuit...
Compiling Recursive Calculator ZK Applications / ZK Circuit...
Creating a proof for the base sum...
The result of 10 + 20 = 30
The result of the recursive sum is 130
```

If you are following, clone the [Github.com/Recursive-zkSNARKs-Calculator](https://github.com/iluxonchik/recursive-zkSNARKs-calculator/) repository, compile the code into Zero-Knowledge circuits, and execute it. You can do so by running the following commands:

```bash
git clone https://github.com/iluxonchik/recursive-zkSNARKs-calculator.git
npm run build
node run build/src/Calculator.js
```

#### Zero-Knowledge Execution Analysis

The code above begins by compiling both of the cirucuits. This is necessary in order to generate the prover and the verification keys for the circuits. The prover key is used to generate the Zero-Knowldge proof, and the verification key are used to verify it. Each prover/verifier keypair is unique per a Zero-Knowledge circuit. The same circuit should always output the same verification key.

The code procceds to create two proofs:

1. A proof for the base sum. This is a plain zkSNARK circuit. This proof is generated by the `BaseCalculatorCircuit`, and it receives two numbers as public input. The code creates a `NumbersToSum` object, and passes it to the `BaseCalculatorCircuit.add()` method. The method returns a `BaseCalculatorCircuitProof` object, which is the proof of the execution of the `BaseCalculatorCircuit`. The code then verifies the proof, and extracts the public output from it. The public output is the sum of the two numbers from the `NumbersToSum` object.

2. A recursive sum proof. This is a recursive zkSNARK circuit. This proof is generated by the `RecursiveCalculatorCircuit`, and it receives a number as public input, and a proof of the execution of the `BaseCalculatorCircuit` as a private input. The code creates a `RecursiveCalculatorCircuitProof` proof, and passes it to the `RecursiveCalculatorCircuit.add()` method. The method returns a `RecursiveCalculatorCircuitProof` object, which is the proof of the execution of the `RecursiveCalculatorCircuit`. The code then verifies the proof, and extracts the public output from it. The public output is the sum of the number from the public input, and the sum of the two numbers from the `BaseCalculatorCircuitProof` object.

### Assertions Offered By zkSNARSs and Recursive zkSNARKs

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-4.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

In order to design and develop sound Zero-Knowledge applications, it is necessary to understand the assertions
that are offered by each part of the aplication, as well as when several assertions are combined together using
recursive zkSNARKs.

As such, let's analyze what eactly is asserted by each of the proofs generated by the corresponding circuits:

1. The `BaseCalculatorCircuitProof` asserts that the sum of two numbers, `A` and `B` is `C`. Since `A` and `B`
   are **public inputs**, and `C` is a **public output**, all of the data related to the sum is exposed in the proof.
   The verifier know exacly which numbers `A` and `B` where used in the sum, and that the result of their sum is `C`.
   The design of the circuit gurantees that `C` is the result of performing an arithmetic addition operation on `A` and `B`. If `C` is not the result of adding `A` and `B`, then the proof verification will fail.

2. The `RecursiveCalculatorCircuitProof` asserts that it has received a `BaseCalculatorCircuitProof` and that proof was valid.
   It then asserts that it extracted the sum from the public output of the proof, added it to the number that it received as
   a public input and that the result of that addition is the public output of the proof.

### Recursive zkSNARKs: What Is Visible To The Verifier?

When desigining Zero-Knowledge applications, it is important to understand what exactly is visible to the verifier, as a
mistake in a single exposed value can either lead to the complete compromise of the facts (secrets) that the Zero-Knowledge application is supposed to hide, or create a Zero-Knowledge proof that proves nothing.

Let's begin by analyzing what information is exposed in the `BaseCalculatorCircuitProof` proof. The `BaseCalculatorCircuitProof` proof is generated by the `BaseCalculatorCircuit` circuit. The `BaseCalculatorCircuit` circuit receives two numbers as public input, and outputs their sum. As such, the `BaseCalculatorCircuitProof` proof exposes the following information:

- The two numbers that were used as public input to the `BaseCalculatorCircuit` circuit.
- The sum of the two numbers that were used as public input to the `BaseCalculatorCircuit` circuit.

The siutation is not so clear when `BaseCalculatorCircuitProof` proof is passed as a private input to the `RecursiveCalculatorCircuit.add()` method? What exactly does the verifier see in the `RecursiveCalculatorCircuit.add()` method? Can the verifier extract the public input to the `BaseCalculatorCircuitProof` proof that was provided to it as a private input? Or is it only able to see the public output of the `BaseCalculatorCircuitProof`?

It's important to note that the entity executing `RecursiveCalculatorCircuit.add()` is both the prover and the verifier, because they are
verifying a zkSNARK proof that was passed to it as a private input, and the prover, because they are generating a zkSNARK proof
themselves (the `RecursiveCalculatorCircuitProof` proof). For the remainder of this discussion that follows, we will refer to the entity executing `RecursiveCalculatorCircuit.add()` as the **verifier**, because we are interested in what is exposed during the verification of `BaseCalculatorCircuitProof` proof in `RecursiveCalculatorCircuit.add()`.

Before you proceed further, take a moment to think about this. What do you think is visible to the verifier in the `RecursiveCalculatorCircuit.add()` method? What do you think is visible to the verifier in the `RecursiveCalculatorCircuitProof` proof?

## Recursive zkSNARKs Under The Hood

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-11.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

In order to answer the questions above it is important to understad how exactly recursive zkSNARKs are implemented in terms
of the Zero-Knowledge circuit construction that enables them. Afterall, recursive zkSNARKs are implemented using "regular" zkSNARKs.

While recursive zkSNAKRs can be implemented in various manners, there are common design guidelines that most implementation
follow for technical reasons. One of such reasons is the succinctness of the proofs, as the end goal is to have proof verification time constant. As such, I will describe the implementation of recursive zkSNARKs in `O1JS`. It's an
also implementation that many other Zero-Knowledge application frameworks mirror.

### Recursive zkSNARKs in O1S

In `O1JS`, when a proof `P` is passed as a private input into a circuit `C`, the pubilc inputs of `P` are passed as private
inputs into `C`, and the hash of those private inputs is passed into `C` as a public input. Additionally, `C` verifies if
the hash of the private inputs matches the hash passed in the public output, thus creating a commitment to the public
inputs of `P`. In practice, this makes the public inputs of `P` semi-private inputs in `C`, as they are passed as private
inputs, but their hash is passed as a public input.

Creating a commitment to the public of inputs of `P`, ensures that the proof produced by `C` creates a cryptographic
commitment to the execution of `P` with particular public inputs. Thus, any external party can verify **which pubilc inputs were used to produce the proof P, without revealing the pubilc inputs themselves, and keeping the proof size succint**.

In order to verify `P` inside of circuit `C`, it is necessary for `C` to know the verification key of `P`, and use that verification key `K` to verify `P`. If `C` uses any verification key other than `K` to verify `P`, the verification will fail.
Verification key `K` can only verify `P`, and `P` can only be verified by verification key `K`. When `C` successfully
verifies `P` with `K`, it creates a cryptographic commitment to `K` in the proof that it itself produces. Since for each `K` there is only one `P` and vice versa, this means that `C` creates a cryptographic commitment to `P` in the proof that it itself produces. This means that the proof produced by `C` can only be verified by `K`, and that `K` can only be used to verify the proof produced by `C`. Thus, any external party can verify **which circuit exacly was used to produce the proof P**. If the thrid party is provided with the implementation code of a circuit, they will be able to verify wether `P` was produced by that circuit.

As such, the verifier can ascertain the use of a specific verification key and thus, by extension, which circuit was employed to generate a recursive proof. However, they remain unaware of the specific recursive proof used unless the circuit logic is designed to reveal this.

### Recursive zkSNARKs in O1JS Examplified

Let's consolidate our understnading with a more generic example. Assume we have two zkSNARKs proofs with the following attributes:

1. Proof `P`, created by circuit `Cp` has public inputs `A` and `B`, and public output `C`.
2. Proof `Q`, created by circuit `Cq`` has `P` as a private input.

When `P`is passed as a private input into `Cq`, the following occurs at zkSNARKs circuit level:

1. The public inputs of `P` are passed as private inputs into `Q`
2. The hash of the pubilc inputs of `P` is passed into `Q` as a public input

When verifying `P`, `Cq` has the following assertios about `P`:

1. If `Cq` is provided with the the pubilc inputs used in `Cp` to generate `P`, it can confirm or deny wether those public inputs were used to generate `P`.
2. `Cq` knows precisely which circuit was used to generate `P`. If `P` is an output of a recursive zkSNARKs, then `Cq` knows all circuits that were involved in the generation of `P`, no matter how many of them there are. This is the power of succinctness! By extension, if `Cq` is provided with the implementation code of `Cp`, it can confirm or deny wether `P` was generated by `Cp`. The same applies to any other circuit that was involved in the generation of `P`.

### Recursive zkSNARKs Are Semi-Private Inputs

Thus, it is more correct to refer to the public inputs of a recursive zkSNARKs as semi-private inputs. They are passed as private inputs, but their hash is passed as a public input. Of course, it is possible to turn those semi-private inputs into
public inputs by providing the verifier with the values of the public proof explicitly.

## Recursive Proof As Private Input: What Is Visible To The Verifier?

Now that we have a clear understanding of how recursive zkSNARKs are implemented, we are ready to answer the question of what is visible to the verifier when a recursive proof is passed as a private input to a circuit.

Given that unless provided with the public inputs explicitly, **the verifier cannot ascertain the public inputs of a recursive proof**. However, they can ascertain the use of a **specific verification key** and thus, by extension, which circuit was employed to generate a recursive proof. They remain unaware of the specific recursive proof used unless the circuit logic is designed to reveal this.

Returning back to our example, let's analyze what exactly is visible to the verifier in the `RecursiveCalculatorCircuit.add()` method when verifying the `BaseCalculatorCircuitProof` proof.

The `RecursiveCalculatorCircuitProof` proof is generated by the `RecursiveCalculatorCircuit` circuit. The `RecursiveCalculatorCircuit` circuit receives a number as public input, and a proof of the execution of the `BaseCalculatorCircuit` as a private input. As such, the `BaseCalcuclatorCircuitProof` proof exposes the following information to the verifer:

- The sum of the two numbers that were used as public input to the `BaseCalculatorCircuit` circuit, when generating the `BaseCalculatorCircuitProof` proof.
- The source code of the `BaseCalculatorCircuit`, as it it necessary to use the verification key of the `BaseCalculatorCircuit` to verify the `BaseCalculatorCircuitProof` proof.

Given this, `RecursiveCalculcatorCircuitProof` has zero-knowledge commitments to the following:

- The sum of the two numbers that were used as public input to the `BaseCalculatorCircuit` circuit, when generating the `BaseCalculatorCircuitProof` proof.
- The source code of the `BaseCalculatorCircuit`, as it it necessary to use the verification key of the `BaseCalculatorCircuit` to verify the `BaseCalculatorCircuitProof` proof.
- The number that was used as public input to the `RecursiveCalculatorCircuit` circuit.
- The sum of the number from the public input to the `RecursiveCalculatorCircuit` circuit, and the sum of the two numbers that were used as public input to the `BaseCalculatorCircuit` circuit.

# Recursive zkSNARKs in zkLocus: Analysis and Code Examples

<div style="text-align:center; max-width:97%;">
  <a href="https://zklocus.dev/">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-5.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</a></div>

While the code and the examples explored in this blog post illustrate the power of recursive zkSNARKs in small-application
scenarios, they are equally applicable to large-scale Zero-Knowledge applications. In order to deepen our understanding, it
is useful to analyze a real-world Zero-Knowledge application that leverages recursive zkSNARKs.

zkLocus leverages the concept of recursive zkSNARKs, allowing for complex and efficient Zero-Knowledge proofs in geolocation authentication. Here, we delve into the specifics of how recursive zkSNARKs are utilized in zkLocus, followed by a detailed look at the relevant code segments from the zkLocus codebase.

You can find the full source code of zkLocus at [Github.com/zkLocus](https://github.com/iluxonchik/zklocus/), or by going to [zkLocus.dev](https://zklocus.dev).

## The Role of Recursive zkSNARKs in zkLocus

zkLocus is fundamentally built on recursive zkSNARKs, enabling a range of powerful capabilities:

1. **Combining zkLocus Proofs**: Recursive zkSNARKs allow for the combination of individual zkLocus proofs. This enables the creation of complex geographical assertions, such as proving presence within multiple or large areas without expanding the proof size significantly.

2. **Customizable Coordinate and Timestamp Sources**: zkLocus can integrate various sources for coordinates and timestamps, such as APIs, hardware devices, or cryptographic signatures. This versatility is made possible due to the recursive nature of its recursive zkSNARKs architecture.

3. **Efficient Proof Size**: Despite the complexity of the geographical assertions, the proof size remains succinct, thanks to the recursive architecture of zkSNARKs.

4. **Verifiable Sources Embedded in Proofs**: Each proof in zkLocus not only encodes geographical data but also embeds the source of this data. This embedding is achieved because zkLocus's circuits are designed to accept coordinates as recursive zkSNARK proofs, ensuring that the source is part of the Zero-Knowledge proof and verifiable by anyone examining the proof.

## How Recursive zkSNARKs are Used in zkLocus

Now, let's examine how recursive zkSNARKs are utilized in zkLocus for one of its core features: sharing your location with
a third-party, without revealing your exact location. For this, we are going to use zkLocus to prove that a certain GeoPoint (geographical coordinate) is within a Polygon (geographical area) at a specific time (timestamp or timestamp interval), without disclosing the exact geographical coordinates or the precise time. This is achieved through a series of steps involving the creation and combination of proofs, as well as the attachment of timestamps to these proofs. Let's delve into the details of this process, referencing specific code examples from the zkLocus codebase.

1. **Creating a GeoPoint Proof**: Initially, a proof is created to attest to the validity of a geographical point. This proof is essential as it forms the basis of subsequent steps in the zkLocus process. The GeoPoint itself is the public output of a Zero-Knowledge proof, and that proof is passed as a private intput to the circuit verifying if that point is within a polygon.

The simplest possible implementation, which simply returns the GeoPoint that was passed to it as a public intput is as follows:

```typescript
GeoPointProviderCircuit = ZkProgram({
  name: "GeoPoint Provider Circuit",
  publicOutput: GeoPoint,
  methods: {
    fromLiteral: {
      privateInputs: [GeoPoint],
      method: geoPointFromLiteral,
    },
  },
});
```

In a practical sense, this same approach is used by all applications that rely on the user for providing their geographical coordinates.

2. **Selecting a Polygon**: A polygon, specifically a three-point polygon, is chosen to prove that the geographical point lies within it. This polygon acts as a private input in the later stages.

3. **Proving Point in Polygon**: The core of zkLocus's functionality lies in proving that a point is within or outside of a polygon. This is done using the `proveGeoPointIn3PointPolygon` method, where the GeoPoint and the ThreePointPolygon are provided as private inputs. The `proveProvidedGeoPointIn3PointPolygon` method extends this functionality by accepting a `GeoPointProviderCircuitProof` along with the polygon as inputs, showcasing the recursive nature of zkSNARKs in zkLocus.

   ```typescript
   proveProvidedGeoPointIn3PointPolygon: {
     privateInputs: [GeoPointProviderCircuitProof, ThreePointPolygon],
     method: proveProvidedGeoPointIn3PointPolygon
   }
   ```

4. **Timestamp Attachment**: zkLocus also allows for the creation of a proof attesting to the validity of a timestamp interval. This is achieved through the `TimeStampIntervalProviderCircuit`, which outputs a `TimeStampInterval`. The `TimeStampInterval` itself is the public output of a Zero-Knowledge proof, and that proof is passed as a private input to the circuit verifying if that timestamp interval is valid.

The simplest possible implementation, which simply returns the GeoPoint that was passed to it as a public intput is as follows:

```typescript
TimeStampIntervalProviderCircuit = ZkProgram({
  name: "Timestamp Interval Provider Circuit",
  publicOutput: TimeStampInterval,
  methods: {
    fromLiteral: {
      privateInputs: [TimeStampInterval],
      method: timeStampIntervalFromLiteral,
    },
  },
});
```

In a practical sense, this same approach is used by all applications that rely on the user for providing their geographical coordinates timestamp.

5. **Combining GeoPoint and Timestamp Proofs**: Finally, the timestamp proof is attached to the GeoPoint in Polygon proof. This is done by the `GeoPointWithTimestampInPolygonCircuit`, which adds the timestamp to the public output of the GeoPoint in Polygon proof, effectively "attaching" the timestamp data to the original proof. The `proofAttachProvidedTimestampinterval` method performs this functionality.

```typescript
GeoPointWithTimestampInPolygonCircuit = ZkProgram({
  name: "GeoPoint With Timestamp In Polygon Circuit",
  publicOutput: GeoPointWithTimeStampIntervalInPolygonCommitment,
  methods: {
    proofAttachProvidedTimestampinterval: {
      privateInputs: [
        GeoPointInPolygonCircuitProof,
        TimeStampIntervalProviderCircuitProof,
      ],
      method: proofAttachSourcedTimestampinterval,
    },
  },
});
```

The returned `GeoPointWithTimeStampIntervalInPolygonCommitment` asserts both: the source of the timestamp, and that timestamp is related to the geographical point measurment in question.

Here, zkLocus uses recursive zkSNARKs to expand the the knoweldge asserted by the proof. In this case, we are expanding the
existing geolocation proof with timestamp data.

#### Additional Assertions

The code examples above have been simplified for the sake of clarity. Additional assertions, such as ensuring that the timestamp is indeed related to the location proof are desirable. In practice, this either means an intermediate
Zero-Knowledge circuit that verifies the relationship between the timestamp and the location, or a
`TimeStampIntervalProviderCircuit` implementation that embeds the location data in the proof.

## Significance of Recursive zkSNARKs in zkLocus

zkLocus leverages recursive zkSNARKs to create complex, multifaceted proofs that are both efficient and verifiable. This approach enables zkLocus to provide robust privacy-preserving solutions in the realm of geolocation authentication, namely:

- **Verifiable Coordinate Source**: By using recursive zkSNARKs, the source of the geographical point is embedded within the proof. It ensures that the origin of the GeoPoint, whether it is from a hardware device, API, or other sources, is verifiable by anyone examining the proof.

- **Timestamp Verification**: The attachment of the timestamp interval to the GeoPoint in Polygon proof not only adds temporal context to the location data but also ensures that the timestamp's validity is as verifiable as the geographical data.

- **Coordinate Commitment to Timestamp**: The attachment of the timestamp interval to the GeoPoint in Polygon proof also ensures that the geographical data is committed to the timestamp. This means the presence of a cryptographic commitment that explicitly states that the geographical data is related to the timestamp.

- **Efficient and Scalable Proofs**: Despite the complexity of combining geographical data with temporal data, the proofs remain succinct and manageable, thanks to the efficient nature of recursive zkSNARKs.

### Recursive zkSNARKs in zkLocus

In summary, the recursive zkSNARKs architecture of zkLocus enables the creation of complex, efficient, and verifiable proofs for geographical data. The source of each data point is inherently part of the proof, ensuring that the authenticity of the geographical data can be independently verified. This is exemplified in the code examples from the zkLocus codebase, with data structures like `GeoPoint` and `ThreePointPolygon`, circuits like `GeoPointProviderCircuit`, and methods like `isPointIn3PointPolygon` and `proveProvidedGeoPointIn3PointPolygon`.

# Recap: Recursive zkSNARK Proof as a Private Input - Visibility Insights

<div style="text-align:center; max-width:97%;">
   <img src="../../assets/blog/recursive-zksnarks-explained/zkLocus-12.png" alt="zkLocus mascot" style="max-width:100%; max-height:auto;border:none;">
</div>

In this comprehensive post, we explored the intricacies and visibility aspects of recursive zkSNARKs, especially when used as private inputs in zk-SNARK circuits. Here's a summary of the key insights:

- **zkSNARKs Basics**: We began with a primer on zkSNARKs, explaining their role in Zero-Knowledge circuits and applications. zkSNARKs act as the proving mechanism within the structure defined by a Zero-Knowledge circuit, allowing for the generation and verification of proofs without revealing secret information.

- **Recursive zkSNARKs**: We dove deeper into recursive zkSNARKs, which enable the verification of proofs without knowing the original statement. They are crucial in scenarios where proving a series of statements' correctness is required without accessing each statement's details.

- **Visibility to Verifiers**: The main focus was on what exactly is visible to verifiers in recursive zkSNARKs, when a recursive proof is passed as a private input to a circuit:

  - Verifiers see the public output of the recursive proof.
  - They can ascertain the use of a specific verification key, indicating which circuit was employed.
  - The specific details of the recursive proof remain unknown unless the circuit is designed to reveal them.

- **Practical Implementation**: Through a detailed example, we demonstrated how recursive zkSNARKs operate in practice, using an O1JS-based calculator application. The application showcased the creation of proofs for simple arithmetic operations and their recursive verification.

- **Zero-Knowledge Assertions**: We analyzed the assertions provided by zkSNARKs and recursive zkSNARKs, emphasizing the importance of understanding what is exposed to the verifier in the proofs.

- **zkLocus Case Study**: We applied these concepts to zkLocus, a Zero-Knowledge application for authenticated private geolocation. zkLocus effectively uses recursive zkSNARKs to authenticate users' locations while preserving privacy. We dissected its implementation, focusing on combining proofs, customizable data sources, and efficient proof size.

- **Significance in zkLocus**: The recursive zkSNARKs architecture in zkLocus ensures verifiable data sources, efficient proof management, and the embedding of the source within each proof, enhancing data authenticity and privacy.

To conclude, recursive zkSNARKs offer a powerful tool in the realm of Zero-Knowledge proofs, enabling complex validations while maintaining succinctness and privacy. Their implementation in applications like [zkLocus](https://zklocus.dev) signifies a leap in privacy-preserving technologies, particularly in sensitive areas like geolocation.
