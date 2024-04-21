
# Ara Blockchain specification

Medet Ahmetson [milayter@gmail.com](mailto:milayter@gmail.com) \
22 April 2024

<p style="text-align: right">
For my love V. P.</p>


**The goal of Ara Blockchain is to support a sustainable economy of the citizen developers’ community**. As the backbone, the Ara Blockchain helps the community develop and own software, allows software modification by anyone on the Internet without permission, and shares revenue from it when the modified software earns money.

As the citizen developers community, we understand two categories of people on the Internet that interact with each other through Ara Blockchain. The first group is the users who use the software, generate software ideas, and sponsor its development.

The second group is the builders who develop and maintain the software.

As a sustainable economy, we understand that each user and builder gets the effort they put in, including when their effort is used on the Internet by other people. A sustainable economy is against piracy and against the ownership of one party (vendor lock-ins).

The citizen developers community has two kinds of economy between the stakeholders.

The first economy is **incentives**. It is a sponsorship of the software development.

The second economy is **usage**. Depending on the software’s business model, software owners may charge for software. For example, builders may charge per API call, ask for a subscription, or make a one-time purchase.


##### Reason for using Hyperpayment protocol

The sustainable economy of citizen developers is not possible without the open-source community. I will argue that the entire Internet capital works thanks to the open-source community.  The problem with the Internet is that, open-source software doesn’t get the rewards it deserves.

If the users solve their tasks with the open-source software, but if that doesn’t get the revenue then the economy is not sustainable. Without revenue earning, open-source software may be following a path to privatize the software. We already have seen that multiple times.

Ara Blockchain has a mechanism that sponsors open-source software builders for all citizen developer’s software. 

This is achieved through the implementation of Ara Hyperpayment Specification. 

The specifications implement the hyperpayment protocol for a particular domain (medicine, education, e-commerce, etc.). The protocol defines how the parties share the revenue. 

The Ara Hyperpayment Specification is a protocol that includes citizen developers and open-source programs in the economy. 

In the following sections, I will explain the blockchain’s technical details, how the citizen developer economy (**incentives, usage) ** is built on top of the blockchain, and, lastly, how the open-source projects involved in the software are registered. The explanation of how the community owns the software will be scattered along the sections. The way how much the open-source programs get is out of the scope of this paper since the Ara Hyperpayment Specification already described it.


---


# Blockchain

We won’t implement the Ara Blockchain as the smartcontracts on the blockchain platforms such as Ethereum. Since blockchain platforms are isolated networks, it won’t have a mechanism to register the open source softwares.

It is preferable to implement the Ara Blockchain as the specific blockchain using popular SDKs such as Substrate, Cosmos, Link, Avalanche, OpSDK, and many more.

In Ara Blockchain, the **data **and the **code **(a.k.a **smartcontract**) are separated. Meaning the smartcontracts are stateless. This gives us the possibility to make any manipulations over the data. And also fix the bugs in the code without locking the access to the data. The smartcontract platforms where the data are bound to the smartcontracts showed that they are ineffective which causes the loss of all the data.


## Data

In the databases, the data has four operations: CRUD. We won’t add anything on top of that. Therefore, users may use the data using the CRUD operations.

Data has four properties. First, it has value. For example, _28_, _“Hanoi City”_ or <code><em>{name: "Medet Ahmetson," id: 1}.</em></code>

The second is the key to the data. By this key, we can identify where it is located. 

The third property of data is ownership. Only the owner can add, delete, or update the data. Reading the data is public.

Last but not least, the data must have the type. 


## Smartcontracts

These smartcontracts may set specific conditions on how the data is manipulated, and the smartcontracts may also set the security or validations. Since they are stored and executed by the blockchain, the data manipulation will be verifiable by the community.

Besides the data manipulation, smartcontracts aims to create custom payment models and custom hyperpayment specifications.

Unlike other blockchains, however, the ARA blockchain’s virtual machine must be interpreter-based. Thus, the deployed code can be verifiable by the people.

The blockchain platforms already implemented the two types of operations. We won’t add anything new, except we will rename them. The Ethereum’s `call` will be `eval`, and Ethereum’s `send` will be `mut_eval`. The `mut_` prefix is short of _mutable_ and means that this code updates the database state. The `eval` on the other hand won’t update the state.

In Ara, the smartcontracts always implement the interface. When you deploy or call the smartcontract, the smartcontract must have the interface that it implements. The interfaces are like data types, they describe the API the code follows and limit the scope of the code.

Since the smartcontract interfaces and the codes are stored in the database and loaded by the interpreter, the operations applied for the data (CRUD operations) also apply to the code.


## IDs

The ID is the special string that identifies the piece of data in a whole blockchain. Its structure is:


```
<type>://<key>/<property>?param=
```


We use the ID to access the data. Additionally, we can define optional parameters to filter it out.


### Built-in types

Ara Blockchain has predefined types and their key type.



* `issue://bytes(32) `indicates the issues, the key of the issue is a 32 bytes long hash.
* `impl://bytes(32) `is the Implementation ID, and the key is 32 bytes long hash.
* `purl://string(1024) `indicates the open-source package, the key is the [https://github.com/package-url/purl-spec](https://github.com/package-url/purl-spec) of the package.
* `hyper://spec/bytes32` indicates the hyperpayment specifications, the key is 32 bytes bytes-long hash.
    * `hyper://spec/ara` - is the preserved data type of the Ara Hyperpayment Specification.
* `hyper://tree/bytes32`—indicates the list of packages and software that share the distributions according to the hyperpayment specification. The key is 32 bytes-long hash.
* `hyper://code/bytes32` - indicates the rules of the specification that calculate how the money is distributed.
* `type://&lt;bytes32|Type|string> `type itself.
* `user://bytes32` is the user address.
* `tx://bytes32 `is the transaction.
* `block://uint64 `is the block.

To see the data structure of each type, you can access `type://tx`, `type://block,` etc.

Before creating data, the developer must define its type. The type key becomes the protocol of the data in the ID:

`&lt;user_type>://&lt;data_key>`.

The predefined data types of them are defined in Appendix I.


## Transactions

To interact with the blockchain, a client must send a transaction:


```
OP ID
tx://?signature=0xhash
hyper://spec/ara/categories/business=ID

Body
```


The first line defines the operation and what to implement the operation.

The `OP` must be one of the following:



* `CREATE`
* `READ`
* `UPDATE`
* `DELETE`
* `EVAL`
* `MUT_EVAL`

The definition of the transactions and their data formats are defined in Appendix II.


### Following the first line, comes the headers. Headers are passed to the blockchain node. 

The first header `tx://?signature=hash`, tells the node, to generate the message with the transaction. It is used to identify the user. Because the signature is the hash of this transaction.

The second optional header tells us to use the following ID as the additional business shareholder from the user’s transactions. If not given, then it will use the default one which is the Ara Foundation’s ID.

Each header covers a single line and ends with the `\n`.


### The last line's `Body` is the transaction's parameter. It must be a code snip that returns the data type accepted by the `ID` in the first line. The ID will be executed with the `Body`.

The Body is separated from the headers by a single empty line.


### Transaction Result

The Ara Blockchain follows the web for handling the data manipulation. 

Here is the transaction result:


```
HTTP_CODE OP ID
type://

Body
```


The first parameter indicates the status of the transaction. The successful transaction must return `200`.

For the errors that occurred by the blockchain node, the blockchain must return a `500` error code. For example, if the header is invalid, or the user doesn’t have permission.

The `404` code is returned if it tries to access the data that doesn’t exist.

The `304` code is returned if the code is asynchronous. It simply means the code is moved out. For example, if the code invokes an Oracle and must continue to work after the Oracle response, or if the code requires multiple signatures to complete.

The smartcontracts may return any other code between `300` and `499`. The `200` and `500` are defined by the blockchain oracles only.

The response’s first header indicates the type of the Body. The body is the result of the execution. 


### Pipelines

The Ara blockchain defines a way to execute the code before and after the data access or smartcontract execution.

The execution goes as the series of the smartcontracts organizing a pipeline.

The pipeline that is called before the access to the data is called a proxy.

The pipeline that is called after the access to the data is called an extension.

Proxies are valid for the validation check, postponing the transaction call, calling the additional data, converting the data, and many other things. Extensions can be used, for example, to clean out the temporary data after execution.

Each data access may have multiple pipelines. For example, you may call one proxy when user A calls `CREATE`, and another proxy pipeline may be called when user B calls the `UPDATE`.

Ara blockchain uses pattern matching to identify which pipeline to call.

If the transaction mutates the database state, then the pipes may be mutating. Otherwise, the pipes must not mutate the database state.


#### Pattern Matching

The data is always defined by the data type. All the data types automatically extend the base data type. The basic data type defines the list of proxies and extensions in the following format:


```
Type {
	proxy: Map<ID, Map<ID, List<type://Pipe>>>;
	extensions: Map<ID, Map<ID, List<type://Pipe>>>;
}
```


The first ID is the data ID. The second ID is the caller ID. If the data is accessed by the user, then it’s the user’s address. If the data is accessed by the smartcontract, then it’s the smartcontract ID’s including the function name.

The last List is the series of the smartcontracts to execute in order. 

Then, this ID is compared against the pipelines.

First, it checks for all custom prefixes that are not built in. If the prefix matches the user’s prefix, it checks the keys. If the key matches, then it checks the path. 

If no keys match, it looks for built-in types except the type:// prefix. Then, the order goes from prefixes to keys to paths. For `issue://` and other such kinds of data, it simply iterates their owners until it won’t reach their owners whose ID prefix doesn’t match the pattern matching ID’s prefix.

Lastly, it checks the type. Then, it checks whether the type of this ID matches the type pattern matching the ID’s type.


## Ownership

All data rows may have owners. Owners are defined in the base type.

The data types and data parameters can not have owners. 

When you create a row, the Ara blockchain executes an internal extension. This extension adds the signer as the owner and a built-in proxy to the proxy pipeline.

Then, any operation on the row or its parameters goes through the added proxy. This proxy validates the access permissions. The proxy will refuse non-owners access if the operation is anything but READ or EVAL.

You may change the owner or use another proxy to overwrite the ownership check using the `UPDATE` operation.

To make the owner as everyone, simply remove the ID. In that case, no proxy will be executed. To execute the proxy for any transaction, simply set the ID as `type://Type` as any type extends it. Therefore this ID will be executed as the default.

To make the pipeline to be called from the users, but not from the smartcontracts, simply use the `user://` prefix as the ID.


# Tokens

Ara comes with the built-in token data type, which is defined as `type://Token`. If you want to create a new token, simply create an instance of the data type.

The basic principle of tokens is the transfer of tokens from one account to another.

We can’t use it for this Update, as it will require two updates. Instead, we need to use smart contracts.

Here is the `Transfer` interface:


```
TransferType = Type {
	token: type://Token
	to: ID;
	amount: bigint;
}

Transfer = code {
	main<TransferType>(Msg m, TransferType params) {
		from_balance = m.signer;
		to_balance = params.token.balances[to];

		update from_balance as from_balance - amount;
		update to_balance as to_balance + amount;
}
}
```


Whenever you want to transfer a token, you may transfer from a user to another user by calling Transfer smartcontract:


```
MUT_EVAL 0xcontract_hash://Transfer
0xhash

{
	token = 0xtoken_type://0xkey;
	to = 0xuser://satoshi;
	amount = 100;
}
```


The token data types have a special proxy that blocks the `Update` from calling directly by the user. That means only the Transfer function has the right to call updates internally.


```
CREATE 0xtoken_type://0xkey/proxy/type://
0xhash

{
	List<interface://Pipe>[0xpipe://NoUpdate]
}

NoUpdate = code {
	main<T>(Msg m, T t) -> T {
		if m.op == UPDATE {
			return error://404;
    }
    return t;
}
}
```


But we need to give permission so that Any contract that follows the transfer is allowed to call update:


```
CREATE 0xtoke_type://0xkey/proxy/0xTransferInterface://
0xhash

{
	List<interface://Pipe>[0xpipe://NoBlock]
}

NoBlock = code {
	main<T>(Msg m, T t) -> T {
		return t;
}
}
```


Now, if UPDATE is called by the anyone denoted here as `type://Type`, then `UPDATE` will be prevented. But remember that `type://` prefix is least important in the pattern matching, therefore, if the caller is of` TransferInterface://`smart contract, then the Update will be given.


# Payments

Payments are smartcontract agreements on how the tokens are transferred between stakeholders. It is generic means it could be N-to-N parties transferring any kind of tokens.

Ara blockchain comes with payment smartcontract interfaces and data types. Any software that wants to charge for usage may handle payments through the smartcontract interfaces.

Here is the Payments data type:


```
PaymentDetails = Type {
balance: Map<0xtoken_type://, bigint>;
	price: Map<0xtoken_type://, bigint>;
	whitelist: List<0xtoken_type://>;
	hyper: hyper://spec/ara;
}
Payment = Type {
	payment: Map<ID, PaymentDetails>; // Key = receiver, value = what to receive
}
```


Any software or user who wants to receive a payment creates a new payment row in the `payment` map. But there is one exception. Remember that for the implementations the payment is defined during its registration? The payments have a special proxy that prevents the creation of the payments for the implementations. The implementations are set only by the node operators internally. The node operators will set the owner of the payment data as the implementation. Which means the implementation owners are the owners of this type.

You may update the `payment.whitelist` to whitelist the tokens. And a proxy that checks is the deposited token is whitelisted.

The payments just like the tokens have a proxy that calls it from the Payment interfaces only.

Here is the payment interface:


```
Interface {
	mut deposit(ID customer, ID business, ID token);
	mut withdraw(ID business, ID token);
	withdrawable(ID business, ID token) -> bool;
	subscribed(ID customer, ID business, ID token) -> bool;
}
```


The `hyper` parameter specifies the payment specification to use for this particular payment. The tree for this payment is then taken using `hyper://tree?root=ID&spec=PaymentDetails.hyper`


# Hyperpayment

The hyperpayment first must define the specification. Then, whenever a payment contract is used, the hyperpayment may be added to that as well.

In order to create a new hyperpayment specification call the:


```
CREATE hyper://spec
```


The hyper protocol receives a Hyper. Here is the variant for the ARA protocol:


```
Type {
	Categories = Map<string, hyper://code>{
    "Business" = hyper://code;
     "Dep" = hyper://code;
     "Customer" = hyper://code; 
     "Environment" = hyper://code;
     "Network" = hyper://code;
     "Operator" = hyper://code;
    };
    $ = [
    	"Customer"; 
    	"Business";
    	"Environment" // 0.5% of customer
    	"Dep" // Shares for the dependencies
    	"Network",
    	"Operator"
    ];
    Splines = {
    	1 = Map<$, List<Categories://>{
    		$Customer, {
    		Categories://Customer, Categories://Business
    }};
    2 = Map<$, List<Categories://>{
    	$Environment, {
    	Categories://Business, Categories://Environment
    }};
    3 = Map<$, List<Categories://>{
    	$Business, {
    	Categories://Business, Categories://Dep
    }};
    4 = Map<$, List<Categories://>{
    	$Dep, {
        Categories://Dep, Categories://Dep
    }};
		5 = Map<$, List<Categories://>{
			$Network, {
			Categories://Customer, Categories://Network
        }};
        6 = Map<$, List<Categories://>{
        	$Operator, {
        	Categories://Customer, Categories://Operator
        }};
    };
    Proxies = {
    	Splines://1 = List<Splines://>{
    		Splines://3
    };
    Splines://3 = List<Splines://>{
    	Splines://4
    };
    Splines://4 = List<Splines://>{
    	Splines://5
    };
    };
    Extensions = {
    	Splines://1 = List<Splines://>{
    		Splines://6
    };
    	Splines://3 = List<Splines://>{
    		Splines://2
    };
    }
}
```


The hyperpayment defines three data types. First is the category of the users. Second is the resources, and lastly the flow of resources between the categories. The direct flow between categories is using the splines. The first spline is always the one that is called first.

Then, we use the proxies and extensions to understand when to call the next spline. Creating the above specification will define a new hyperpayment. Now, any project may add a tree that matches the hyperpayments.

Note that each category defines a payment smartcontract. Then, hyperpayment will call the `deposit` function in that smartcontracts.

–

TREE


```
CREATE hyper://tree?spec=hyper://spec/hash

Type hyper://tree {
	string: Map<ID, hyper://tree>;
}

Map <ID, hyper://spec_hash/tree> {
	Business = Map<ID, hyper://tree> {
		ID = hyper://spec_hash/tree {
			Environment = Map<ID, hyper://spec_hash/tree> {
				purl://Python = hyper://spec_hash/tree {};
				purl://Dockerfile = hyper://spec_hash/tree {};
        };
        Deps = Map<ID, hyper://spec_hash/tree>{
    		"pkg://pypi/glom" = {};
    		"pkg://pypi/fqdn" = {};
    		"pkg://pypi/dnspython" = {};
    		"pkg://pypi/fastapi" = {
    			Deps = Map<ID, hyper://spec_hash/tree> {
                    "pkg://pypi/pydantic" = {
             			Deps = {
                            "pkg://pypi/typing_extensions"
                		};
                    };
                    "pkg://pypi/typing_extensions";
                }
            }
        }
        }
}
```


Note, that the ID has the parameter as `spec`. This will give the list of possible categories.

Once we have the specification at `hyper://spec/hash` and a project tree at `hyper://tree/hash`, the next thing is to implement the payments.


```
CREATE hyper://code?spec=hyper://spec/0xhash

Interface {
	resource(bigint initial_amount, hyper://Tree tree, string category, uint level) -> bigint;
	resource_per_leave(bigint initial_amount, hyper://Tree tree, string category, uint level) -> bigint;

	deposit(Token t, bigint initial_amount, hyper://Tree tree);

	withdraw(Token t, bigint initial_amount, hyper://Tree tree);
}
```
The above code will create the hyperpayment smartcontract for the particular specification. 

The interface parameters are:


The `resource_per_leave` function is identical, except that it divides the sum by the number of keys in the category.


The deposit is a private function similar to mut_eval_hyperpay, except it locks the tokens internally without transferring them to the user. 


The deposit will:

* Internally it will call the `resource_per_leave` and then call the categories payment contract.
* If no tree, then all the sum will be given to the parent.

Note that this function may be called for all categories, which will specify the rules for calculating money for each category.


`The withdrawal` will deposit all the tokens in this hyperpayment. This function must be called manually; it’s not called automatically by smart contracts or other functions.


# Production and Incentives Economy

An issue is the user’s desired idea, feature, bug, or modification. Issues are attached to the list of URLs that act as issue tags. A single URL or series of URLs may have many issues.

The implementation is the solution for the issue. One issue may have many implementations.

Users are creating the issue. Developers are creating the implementations.

Users and developers discuss issues and their implementation outside of the blockchain. However, the blockchain handles the economy of issue implementations.

Users must attach the incentives along with the issue. Any implementation is given in the test mode. Once the implementation satisfies the user’s needs, the attached incentives are distributed to the developers.

If the implementation charges money for usage, developers must attach the usage payment to the. After it receives the incentives and is marked as a test passed, the developers may use the payment for usage.

Incentive Flow:



1. Add new issue
    1. Update the issue
    2. Increase incentives \

2. Push the implementation
    3. Update the implementation \

3. Implementation passes the test.
4. Push the production version and get incentive rewards

The incentives are handled by the built-in smartcontract: `Incentives`. This smartcontract implements the `Vault` interface. The vault interface simply stores the data deducted from the users. And only the owners of the issue could withdraw them. 

**The production flows are end-user called only. The smartcontracts can not call it.**


### Production API


#### Creating issues

The following transaction adds a new issue for the websites. 


```

CREATE issue:// 
0xhash 

{
	websites: string(1024)[5],
      title: string(1024),
    document: string(3Kb),

    incentives: { address: { value: bigint, token: address } }
```


`}` 

The `params.incentives` must have one parameter with the user’s account address. The user must have the tokens.

The Ara blockchain has built-in pipelines for the issue creation.

The proxy pipeline checks that the user has enough tokens in the `incentives`. The second proxy makes sure that the user doesn’t provide the ownership parameter.

The extension pipeline, first calls the `vault://deposit` for the incentives.

The second extension sets the incentive as the owner of the issue.

The last extension simply deletes the incentives element from the issues.


---


#### Updating issues

The Ara blockchain has a built-in proxy for updating the issues.

Issues can set the title or incentives. Updating the issue’s incentives is ignored.


#### Like

To increase the incentives or support the incentive as a new user, simply add the token to the `incentives` smartcontract for the particular issue by calling the `deposit`.

The incentive smartcontract has a built-in proxy that allows sending tokens only by the issue's owners for deposit.

The incentive smartcontract has a built-in extension, that sets the incentive provider as the owner of the smartcontract for deposit.

The incentive smartcontract has a built-in extension that prevents updates of the smartcontract proxies, ownership, and extensions.

Users may update the proxies, and extensions for the particular path.


#### Creating and updating implementations

The following transaction example creates a new implementation


```

CREATE impl://
0xhash

{
		issid: string,
      	source?: Source,
    payment?: {
     		Type: address,
          Value?: bigint
     },
    distributions: { address: string|bigint },
	constructor: string(10Kb)
    hyper: hyper://spec/ara
}
```


The `params.payment` is the reference to the smartcontract of the payment interface. Optionally, the user may specify the hardcoded price of this service.

The `params.source` is the reference to the project's source code. The source code must be the Git or URL. The address is optional, and the user may not give it.

Lastly, the `params.distributions` is the list of the accounts that own this particular implementation. In the case of a string, the value of params.distributions could be a percentage. Distributions must have at least one address.

The successful result must return the ID of the generated implementation. The implementation ID is the hash of the issue ID and source converted into a stream of bytes. It is unique for the entire blockchain.

Creating implementation has built-in proxies and built-in extensions.

The built-in proxies make sure that payment is valid.

The second proxy makes sure that this transaction didn’t set the user’s owner.

The first built-in extension sets the distributions as the owners of the implementation.

The second built-in extension makes sure that payment is valid.

The third extension makes sure that payment is created for this implementation.

The last extension makes the owner of the payment the implementation.


---

The Ara blockchain has built-in proxies for updating the implementation.

First, if the phase is “test”, then only test parameters are updated, and only by implementation owners.

If the phase is “prod”, then only prod parameters are updated, and only by implementation owners.

If the updating the phase, then only from test to prod, and only by the issue owners. 

The Ara blockchain has built-in extensions for updating the implementation.

If the updating phase is, then it also calls the withdraw function in the `incentives` for the issue.


## Add Hyperpayment for incentive distributions

When the users change the implementation phase from “test” to “prod,” the Ara executes a built-in extension pipeline that pays out the incentives.

The first is the call the withdraw and put it into the balance of the implementation. Then, it will call the `hyper://code/deposit?spec` specified in the implementation. The last parameter will call the withdraw function of the hyperpayment.

The tree is taken from the `hyper://tree?root=ID` where ID is the implementation ID.


# Tracking environments and dependencies

The Ara Blockchain must keep the package manager URLs supported by the purl.

When a builder creates the implementation, the Ara Blockchain must generate its Software Bill of Materials (Bom) in the `CycloneDX` format. The bom is used as the `dep` category projects.

When a builder creates the implementations, using the GitHub Linguist, the Ara Blockchain must identify the programming languages used for the implementation. The identified programming languages are stored in the `environment` category. The environment projects are added as the empty purl in the following format: `purl://env_name?category=true`

When an implementation is updated, the BOM and Programming languages are updated and over-writes the respectful categories.

Using the Bom, the packages are registered in the `purl://`.


## Registering the software as a citizen developer’s project.

The `purl://` has a special proxy that allows to creation or update of the package. If the package is called by anyone other than the `Incentives` smartcontract, then `dependencies `generation and `environment` generation are executed automatically. 

To specify a custom hyperpayment specification, it can be passed as the ID argument:

`CREATE purl://?spec=` 

There is only one Network, and it’s inserted by the Ara Blockchain.


## Setting environment owners

When the environment purl is created, it’s the owner of the blockchain itself. The Ara Blockchain must have a platform that allows owners of the programming languages to set themselves as the owners. It is manual and any changes to the programming language must be voted by ⅓ of the node operators.

Once the owners set, then any changes to the environments, won’t require the ⅓ of the node operator’s vote.


## Setting dependency owners

When a purl is added, it won’t have the owners. To simply add the owners, simply push an update that changes the `owners`. There is a special proxy that is executed when only owners are updated.

The Ara Blockchain checks the source code of the package. There must be `ara.json` in the root. The `ara.json` lists the addresses:


```
{
	Owners: List<ID>{user://};
}
```



## Appendix I. Built-in Data Types


### Types


```
type Issue = {
title:         string[1024],
document:      string[3Kb],
websites:      string[],
author:        address,
incentives:   { 
    address:  { 
               value: bigint, 
               token: address 
    } 
}
previous_issid?: string
}

type Implementation = {
		issueid: string,
		implid:  string,
	phase:   string,
    prod?: {
         constructor: string[10Kb],
         source:      Source
	},
    test: {
       constructor: string[10Kb],
       source: Source
    },
    payment?: {
     		Type: address,
          Value?: bigint
     },
    distributions?: { address: string|bigint }
} 

type Source = {
url: string[1024],
branch: string[1024],
commit: string[1024]
}

type Msg = {
	signer: bytes32,
	from: ID,
	mode: C|R|U|D|E|M
}
```


All data types extend the base data type which is: 


```
type Type = {
proxy: Map<ID, Map<ID, List<type://Pipe>>>;
extension: Map<ID, Map<ID, List<type://Pipe>>>;
Owners: List<ID>;
}
```


This data type is defined as `type://type.` 

The proxy interface is


```
interface Pipe = {
	main<T>(Msg msg, T t) -> T;
}
```


The token datatype is reserved as `type://Token`.


```
Token = Type {
	balances: Map<ID, bigint>;
}
```



## Appendix II: Transactions and their formats


### CREATE

CREATE adds a new element to the database. There are three possible ways to use CREATE: first, creating a new data type or smartcontract interface; second, creating a new row of the certain data type or smartcontract interface; and lastly, adding a parameter to the element.


#### Creating data types and interfaces


```
CREATE type://
0xhash

Type {
 Name: string;
}
```


An example of creating a new datatype:


```
CREATE type://
0xhash

Interface {
	foo(string) -> bool;
}
```


The example smartcontract interface.

In the examples above, the ID in the first line of the transaction contains only the prefix `type://`.

Because, this command itself generates the new key that is returned by the transaction.

The key must be the parameter code using the keccak256. If the type exists, it must throw an error and return a 500 status code.

The above example for data type creation generates `b47f0aa440d730935949cc68e77cdcb344bc61debd054cdec19ffab376879633 `datatype.

The above interface will generate the following key: `aa4ca2b3710bd035ab32fc3dec57a7ef1632d28092ce618e115db6aa59c31818`


#### Creating the rows

Now, instead of the `type://` you can use the generated keys to insert new rows.


```
CREATE b47f0aa440d730935949cc68e77cdcb344bc61debd054cdec19ffab376879633://
0xhash

{
	Name: "Medet Ahmetson"
}
```


The above code will insert the above data as a row. The create command generates a unique key using the user’s signature, params and type combined.

The above example will generate a new row whose key is:


```
efee8aa4e1bf5ea60bfd66d017339d56cf39598dd6033d96cca0677ff6a9b811
```


Combing it with the type will generate the ID of the row that you can access as well:


```
b47f0aa440d730935949cc68e77cdcb344bc61debd054cdec19ffab376879633://efee8aa4e1bf5ea60bfd66d017339d56cf39598dd6033d96cca0677ff6a9b811
```



#### Creating property

The Ara supports lists and maps. To add a new element to either, you can use the CREATE command.

Suppose we have the following type:


```
Type {
  list: List<int>;
  map: Map<string, int>;
}
```


The type is deployed as `0xtype`, while we set a new row with an empty list and map with its key as `0xkey`.

Then


```
CREATE 0xtype://0xkey/list
0xhash

1
```


Will add the `1` as the element of the list. For the list at the end.

The lists have the built-in command that, for now, it is only `shift`.


```
CREATE 0xtype://0xkey/list?shift=true
0xhash

2
```


The above example sets 2 as the first element. Now, the list is `[2, 1]`.

The create command works in the same way for the maps.


```
CREATE 0xtype://0xkey/map
0xhash

{
  "Medet": 123
}
```


The above example will set the medet as the new key.

Note that, the key must not exist in the map. Otherwise, it must return an error with the 500 HTTP status code.


### READ

The READ command reads the data from the database and returns it back to the user.

The format is:


```
READ ID
```


If the read doesn’t contain a key, but only prefix, then the command returns a list.

The ID can be the particular property as well.


```
READ 0xtype://0xkey/param_1/param_2
```

The example above will return the `param_2` in the key. It may be the internal type’s property. 

For the list, the last parameter is the number indicating the index of the element.

`READ 0xtype://0xkey/list/1` - returns the element at index 1 in the `list`.

For the map, the last parameter is the key name:

`READ 0xtype://0xkey/map/Medet` - returns the value of the `Medet` element in the map.

The READ transaction has the `list `parameter. With this parameter, the reading will return multiple data. 


```
READ 0xtype://0xkey?list
```


The result will return READ_LIST type. The type is:


```
READ_LIST = Type {
	page: uint;
	length: uint;
	data: List<T>;
}
```


Along with the list you can also specify the `page` and `length`.

The length could be a maximum of 500 in length, while at default it returns 50 elements.

If the page doesn’t have the data, then the list is empty.

Reading the element or list of elements can be done through filtering:


```
READ 0xtype://

code {
func main() -> CMD {
    return SELECT * WHERE "url_1" OR "url_2" IN websites;
    };
}
```

Filtering accepts the special interface which is the `main() -> CMD`

The above example passed the anonymous contract that checks and selects the element whose websites contain `url_1` or `url_2`.


### UPDATE

This transaction updates the value of the data. It can update the entire row or one of the parameters of the row.

Calling this command with an ID that has only a prefix is prohibited, and it must return a 500 status error.

Calling this command with the `type://` prefix is prohibited and must return a 500 status error.

Calling this command for the ID that doesn’t exist must return a 404 status error.

Since updating the data mutates the data, the user must provide the signature.


```
UPDATE 0xtype://0xkey
0xhash

value
```
The above transaction updates the `0key` with the `value`.

You can also update the list element; in that case, the last parameter must be a number indicating the element's index.


```
UPDATE 0xtype://0xkey/list/1
0xhash

2
```


The above transaction updates the list element at index 1 with the value 2.

You can also update the map element, in this case, the last parameter must be the string indicating the map element:


```
UPDATE 0xtype://0xkey/map/Medet
0xhash

321
```


The above transaction updates the value of the `Medet` element in the `map` to `321`.


### DELETE

The DELETE command deletes the row or parameter of the row. Deleting the property simply resets the value to 0. If the property type is a list or map, then they will be cleared out entirely.

For the list, it simply shifts the entire list to the front.

For the map, the element is deleted from the map.

Calling this command with the `type://` prefix in the ID is prohibited and must return a 500 error status.

Calling this command with the type prefix without the key in the ID is prohibited and must return a 500 error status.

Since deleting updates the state of the database it must be provided with the signature hash.


```
DELETE 0xtype://0xkey
```


The above code deletes the row at key `0xkey`.


```
DELETE 0xtype://0xkey/param_2
```


The above code deletes the row’s `param_2` parameter. For the numbers, the value is reset to 0. For the strings, it is an empty string. If the `param_2` is a map or array, then it is emptied. If the parameter is another type, then all parameters of that type are reset to 0 recursively.


```
DELETE 0xtype://0xkey/list/2
```


The above example deletes the list element at index 2. If the list has other elements after that index, then they are shifted to the front.


```
DELETE 0xtype://0xkey/map/Medet
```


The above example deletes the `Medet` element in the map.


---

When you create a type, it must be a struct based on the primary `Type`.

The primary type comes with additional properties which are called `Proxy` and `Extension`.

The proxy is called automatically before the data is accessed, and the extensions are called automatically at the end of the transaction.

They are a map of keys to the list of `Pipe` interfaces:


```
Type {
	Proxy: Map<ID, List<Pipe>>;
	Extension: Map<ID, List<Pipe>>;
	Owner: List<ID>;
}
Pipe = Interface {
	main<T>(Msg msg, T t) -> T;
}
```


Additionally, the primary type comes with the owner list. All the data operations can also set the primary object parameters in the transaction to overwrite the default values.


---


### EVAL

The EVAL transaction executes the function and returns the result. The syntax is:


```
EVAL ID

params
```


Here, the params is the list of arguments passed to the smartcontract. The ID must be a contract ID.

Calling this transaction with the `type://` prefix is prohibited and must return a 500 error code.

Calling this transaction without a key is prohibited and must return a 500 error code.

If the ID contains a parameter, then the parameter must be the name of the function.

Nested functions are not allowed. In case the function doesn’t exist, it must return a 404 error code.

If the ID doesn’t contain a parameter, then it must execute the `main` function.

The params is the map of the function arguments. If the argument is not given, then it uses the default value for that argument:


```
EVAL type://bytes32/func_name

{
	arg_1 = "123";
	arg_2 = "213";
}
```



### MUT_EVAL

This transaction executions the function and returns the result. The syntax is:


```
MUT_EVAL ID
0xhash

params
```


Since this function mutates the database, it must be with the user’s signature.

Calling this transaction with the `type://` prefix is prohibited and must return a 500 error code.

Calling this transaction without a key is prohibited and must return a 500 error code.

If the ID contains a parameter, then the parameter must be the name of the function.

Nested functions are not allowed. In case the function doesn’t exist, it must return a 404 error code.

If the ID doesn’t contain a parameter, then it must execute the `main` function.

The params is the map of the function arguments. If the argument is not given, then it uses the default value for that argument:


```
MUT_EVAL type://bytes32/func_name
0xhash

{
	arg_1 = "123";
	arg_2 = "213";
}
```



## Appendix III Smartcontract Syntax

Our programming language is called Aradil.

It’s the simpler form of the typescript. For now, think about it as the javascript.
