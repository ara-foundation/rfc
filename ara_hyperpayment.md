# Ara hyperpayment

_Keywords: opensource_



* Status: draft
* Editor: Medet Ahmetson milayter@gmail.com
* Protocol: Hyperpayment protocol. [Protocol Link](https://docs.google.com/document/d/1d3TgpcZGwnmzO3SXthvqX_hG5cji6Yj3oZUMvrwDm08/edit?usp=sharing).

This is the extension of the [Opensource hyperpayment](https://docs.google.com/document/d/1ccWhLQYm0Yz9tn-i2gi-CMJy4uFYVsTB0qlC6CEookk/edit?usp=sharing).

Ara hyperpayment defines the payment platform where cryptocurrency purchases are made. This specification describes how much the network operators and blockchain developers consume the transaction fee.

> [Google Docs](https://docs.google.com/document/d/1SpYRn30p_mJn_n4Le0mYinui_uJ8OQr1ggVlDUQAbno/edit?usp=sharing)

## License

Copyright © 2024, Medet Ahmetson, and contributors

This Specification is free software; you can redistribute it and modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License or (at your option) any later version.

This Specification is distributed in the hope that it will be useful but WITHOUT ANY WARRANTY, INCLUDING THE IMPLIED WARRANTIES OF MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License and this program; if not, see [http://www.gnu.org/licenses](http://www.gnu.org/licenses).


## Change Process

This Specification is a free and open standard governed by the Ara Foundation. Any changes must be approved by consensus with the main editor.


## Language

The keywords “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119 (see “[Key words for use in RFCs to Indicate Requirement Levels](http://tools.ietf.org/html/rfc2119)").


## Goals

No payment system allows easy distribution of payment using hyperpayment protocol. Blockchain and cryptocurrency are the ideal for implementing hyperpayment.

However, to operate the blockchain, both blockchain networks must take the transaction fees to maintain the network.

This specification defines the revenue model of the blockchain that implements Open-Source hyperpayment, which allows the distribution of tokens to open-source libraries. Our aim for the blockchain is to support any kind of hyperpayment, whether for social goods or something else.

It aims to:



* A universal platform is used to implement any hyperpayment specifications, whether they extend open source or not.
* Paid by the users, not by the software developers.
* Paid to support blockchain development and network security.

The use cases include:



* Ara blockchain for the personalized computers.


## Categories

There are two additional categories of the software:

**network** category is used for the blockchain foundation. The money in the category is used as the payment for the blockchain developers along with all dependencies it uses.

**operator** category defines the class of the addresses that run the blockchain network. The amount on how much it charges depend on the blockchain implementation.


## Categorization

The following section describes how to list the users. Then, how to classify them.



* **network**—is defined by the blockchain foundation. 
* **operator**—is the node that runs the blockchain network they register themselves during the block mining.
* **network**-defines itself as the business here that lists the dependencies and environments.


## Flow


### Resources

- **`$`customer** is the money that is taken from the customer. **$business** was defined in the original open-source protocol. Here the business is only 18%. 
- **`$`network** is the 2% of **$customer** 
- **`$`operator** is the money that is taken from the customer. Used to pay for the node operators.


### Splines



- Flow 1 - **$customer** from **customer** to **business**
- Flow 2 - **$environment** from **business** to **environment**
- Flow 3 - **$business** from **business** to **dep**
- Flow 4 - **$dep** from **dep** to **dep**
- Flow 5 - **$network** from **customer** to **network**
- Flow 6 - **$operator** from **customer** to **operator**


### Junctions

Flow 3 comes before the Flow 1. 

Flow 2 comes after Flow 3. 

Flow 4 comes before Flow 3.

Flow 5 comes before Flow 4.

Flow 6 comes after Flow 1.


## Contracts

Contracts define how the money is calculated for the category and the junctions. It also determines how the money is distributed within the category elements.

**customer** doesn't have a contract, as only the receiving parties can define it.

**network** defines the contract by which it will receive the payments. The network contract shall have three software packages under the business category. One for the client, one for the specifications, and one for the blockchain node. Each business user is required to get an agreement on how many percentages of the tokens are deducted from them.

**operator** contract defines the tokens per transaction call. The price is arbitrary and depends on the computation complexity and network intensity. Therefore, it is up to the node operators how much they would charge.


## Implementations and Further Reading

Define how the alternative client and blockchain node operators may extend this protocol so that alternative software developers can also receive the money.
