    XPOST, from a team sub: http://www.reddit.com/r/ProjectDNET/comments/2g4xsr/g3/

g3, or game
=========

* [Initial Concept](#initial-concept)
* [Feedback](#feedback)
* [Ethercraft](#ethercraft)
  * [How to](#how-to)
  * [Message A](#message-a)
  * [Contract: Winderness](#contract-wilderness)

##Initial concept

Is a contract that has a property #start, which has the value of the first message. Which itself is the first item of a linked list of messages.

Add-ons are created by creating a contract that despite its inner working should return a value that corresponds to a message in the above linked list. 
These return values are considered the possible outcomes of the possible actions of the given player.

To add to the game a player must be informed of which contracts (moves) are possible actions to take, to a greater or lesser degree. This informing is done by adding a message to the contacts linked list.

This way the 'game' what ever that might be will forever be editable, verifiable and truly decentralised. What it would or even could become is beyond me.

#Feedback

From [u/avsa on r/ethereum](http://www.reddit.com/r/ethereum/comments/2g4yke/g3_an_ethereum_game/ckg9xv3):


> I don;t really understand the game.. Is it a "build your own rules", [Nomic](http://en.wikipedia.org/wiki/Nomic) game? Is it a "output the input" [Quine](http://en.wikipedia.org/wiki/Quine) competition?

Having thereby just come across the concept of Nomic games, as described by the author in his [Initial Ruleset](http://legacy.earlham.edu/~peters/writing/nomic.htm) It seems as through the framework can be applied to **g3**. I'm still busy processing the implications of such a framework and what the initial ruleset would be under such a system.

#Ethercraft

Is a prototypicical implementation of the currently evolving idea of **g3**, the game, and considered to be the first implementation but by no means the last, let alone canonical.

![Diagram](https://raw.githubusercontent.com/d11e9/g3/master/g3-xmind.png)

> The above diagram is a first draft of what a g3 node might look like, in terms of `contract state`. This is orthoganal to logic that this contract may or may not impliment otherwise. But it is assumed that the easiest way to modify/add-to a node is my providing a new contract to handle whatever the new action/move may be.

The base node or genesis contract would implement an "extend" action, which would behave something like the contract body below written in pseudo-serpent for simplicity.

    if msg.data[0] == "extend"
        msg_hash = msg.data[1]
        contract_add = msg.data[2]
        contract.storage[msg_hash] = contract_add
        return 0

Obviously with a few more checks, as to the authenticity of the caller and whether anything is being overwitten etc. The node should allow players to move between states using a method similar to the contract below. Which takes a transaction (TX) with the following data attached: `<contract-address> <action>` where action will be forwarded to the called contract and the players state will be updated to that returned from the called contract ( if it exists ), and the contracts call count incremented.
    
    # param 1 contract to call
    if contract.storage[msg.data[0]]
        returned = call( msg.data[0], [..params] )
        if contract.storage[ returned ] == msg.data[0]
            contract.storage[ msg.data[0] ] += 1 
            contract.storage[ caller ] = returned
    

## How to

To start you need a message hash and a contract. Creating a TX with the data: "extend" #contract-address.

Once submitted, a player can call that contract via the genesis contract by creating a TX with data: #contract-address #action [.. params].

The value returned from such a call should equal an existing message hash in the origin contract and the callers address within is changed to this message hash (from this point on thought of as a #state)

The player having just moved states within the context of the genisis node. And also having entered into ideally an initial #state within the the node with which they interacted.

This allows the creation of decentralised nodes representing the current #state of the game at large. Which can easily interact with one another to move players within the game.

##Message A

    The swirling wisps of ether start dissipating just ahead of your field of vision.

    Beyond you can only just make out an empty expanse, a wilderness.

    /enter 

###Contract: wilderness

