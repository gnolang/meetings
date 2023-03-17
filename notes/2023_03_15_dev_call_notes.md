# Wednesday, March 15th - GoogleMeet 

## Summary of the discussion

* We discussed IBC, and how IBC can be shaped to Gno needs
* We discussed different implementations for the event emission system on Smart Contracts (something analogue to Ethereum Smart Contract Events), so users can monitor these events as they happen on the chain
* We discussed the ability for Smart Contracts to interact across Gno chains, and how we could maybe utilize a variation of IBC for that
* We discussed opening a Design challenge to the community

## Ian Notes:

Gnoland Dev Call 3/15/2023

- Petar Dambovaliev Intro
- Musician
- Started with PHP -> Golang -> Rust - Golang
- Very good at optimizations (CPU, RAM, resources)
- Last company built a data store and query language
- Contributions to the Go compiler and Rust compiler

- Maxwell Liu Update

- May start working on contract to contract communication
- Jae gave an update on a channel approach on this
- Go wants independent channels for send and receive
- Propose to use 2 channels one for send one for receive
	- IBC open returns 2 channels
- Function call approach
	- Provide a proxy function on the other chain
		- Risk it's magic, and implies synchronization (this may be a later approach)
- Manfred brought up the Ethereum event concern with how to interface with IBC
- Event emission -> RPC listeners
- Jae looking to send something more generic than an event and let the receiver decide what to do with the data
- Milos event emissions are logged down to an event log (receipts hash of logs), open a subscription for a topic (I want to be notified of any events name/typed "x", filter manager: checks blocks for logs and fires events on the channel, basic pub/sub model)
- Jae: should events be passable over IBC? Milos, Eth has independent event systems for contacts and nodes.
- Jae: Can an event values be used as and input of a subsequent smart contract call? Milos: yes, can inspect values of the smart contract call as a part of the event
- Jae proposes we start the Synchronous implementation for initial experimentation with IBC for Gno
- Manfred What API to call to call external service from GRPC, RPC, manual marshaling or other? Petat and most likes GRPC (easy and performant)
- Jae: How do you share types across chains? No reflect currently. (Chains expected to expose types as a package (protobuf or other)?), Problem with concurrency.
- Do we prioritize Type safety or use something more like reflect
	- Jae: std/libs -> openIBC -> params include send arguments, receive arguments, w/ documented API (ABI compatible?)
	- Manfred protobuf Go->ABI code generator (confio CosmWASM) Protobuf, Amino. Keep API like gRPC,
- Amino: can use reflection, or use transpiler to compile protobuf from Go
- Make systems parallel and compatilble with Protobuf 3 and GRPC. Gen-go
			
- Manfred - News Update


## Chat
*Miloš Živković9:22 AM*
@Manfred 

Solidity events are written down to the transaction receipts in the form of Logs, and they can be parsed async later on, since receipts are part of the blockchain state that's immutable and saved

*Petar Dambovaliev9:34 AM*
If you want to add something quick and dirty before proper implementation, we can expose a data structure that would have a bunch of methods that can be invoked with standard syntax

