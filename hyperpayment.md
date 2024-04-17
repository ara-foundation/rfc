#  Hyperpayment protocol

_Keywords: Ara, Maydan, Hyperpayment_


* Status: draft
* Editor: Medet Ahmetson [milayter@gmail.com](mailto:milayter@gmail.com)

The Hyperpayment protocol defines an agreement on how the payment occurred between multiple parties who don’t know each other.

> [Google Docs](https://docs.google.com/document/d/1d3TgpcZGwnmzO3SXthvqX_hG5cji6Yj3oZUMvrwDm08/edit?usp=sharing)

## License

Copyright (c) 2024 Medet Ahmetson, Sergey Pak, and contributors

This Specification is free software; you can redistribute it and modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License or (at your option) any later version.

This Specification is distributed in the hope that it will be useful but WITHOUT ANY WARRANTY, INCLUDING THE IMPLIED WARRANTIES OF MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License and this program; if not, see [http://www.gnu.org/licenses](http://www.gnu.org/licenses).


## Change Process

This Specification is a free and open standard governed by the Ara Foundation. Any changes must be approved by consensus with the main editor.


## Language

The keywords “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119 (see “[Key words for use in RFCs to Indicate Requirement Levels](http://tools.ietf.org/html/rfc2119)").


## Goals

Hyperpayment is a protocol of a payment between multiple parties. Difference from other payment systems is that, parties are getting into the system without asking a permission, yet will be eligible for the payment by the rules of the domain.

Examples where hyperpayment could be used:
* Revenue of the open source library from the proprietary software.
* Data monetization if user's data is used by third parties.
* Crowdfunding.
* Collaboration over a project on Internet.

Hyperpayment aims to:
* Be the internet’s payment protocol.
* Be minimalistic in terms of syntax.
* It should be trivial to make it understandable by humans and computers.
* Be expandable
* Reduce the risk of human error to as close to zero as possible.


## Specification

Hyperpayment defines a text format of how the particular parties in a domain on the internet interact. The agreement of the specification must be implemented as a computer software. The software must come with the specification reference.

The protocol must specify the

* Category of the parties.
* Identification of participants and how the participants can join.
* The flow of money between categories is the vectors and splines. The first flow must be defined as the initial point of which category sends what. The flow section must describe the junction of the flow vectors.
* The resource defines all the money. The money may be deposited, withdrawn, or transferred.
* The resources denoting the money shall be described as `$` prefix and category. The flow must be based on the resources.
* Junctions must be defined using the hooks *“before,”* *“after,”*.
* Contract of each category that shall define how the resource is received and how it is forwarded

The category must not have the keywords whose lemmas are:

“Ara,” “hyperpayment,” “user,” “category,” and “love.” 

> This means, “users,” “categories,” “categorize,” “ara,” “a-ra” must not be used as the name of the category.

The category must be defined as a single word.

Any hyperpayment protocol of any domain must be expandable. 

The sum of all money that every participant in all categories receives must equal the initial resource sent by the first user.

The expanded protocol 
* must include the web link to the protocol that it expands in the intro. 
* The expanding protocol may add new categories. 
* The expanding protocol must not change the category list defined by the parent protocol.
* The expanding protocol must not change the contract.

Here is the template that the specifications may follow:


    Title: Opensource


    Implements: Hyperpayment protocol ([Link to this document](https://docs.google.com/document/d/1d3TgpcZGwnmzO3SXthvqX_hG5cji6Yj3oZUMvrwDm08/edit?usp=sharing))


    Authors: John Doe ([to@example.com](mailto:to@example.com))


    Intro: the open source library; if it’s used in the proprietary software, then the user must also pay for the library.


    Categories:


        * Client
        * Dependency
        * Business

    Flow:


    Resources: $client


    Flow#1 - $client from Client to Business.


    Flow#2 - $client from Client to Dependency.


    Junctions


    Flow#2 is executed before the flow#1. The flow 2 receives the remaining amount from the flow 2.


    Contracts:


    Client: is defined by the businesses. It’s the user who uses the client.


    Business: is defined by the software developer.


    The business owner defines dependency by exposing the open-source code of their software.


## Implementations and Further Reading

[Opensource hyperpayment specification](https://docs.google.com/document/d/1ccWhLQYm0Yz9tn-i2gi-CMJy4uFYVsTB0qlC6CEookk/edit?usp=sharing).

[Ara hyperpayment specification](https://docs.google.com/document/d/1SpYRn30p_mJn_n4Le0mYinui_uJ8OQr1ggVlDUQAbno/edit?usp=sharing).

Todo: define the interface for the contracts and types of the contracts.
