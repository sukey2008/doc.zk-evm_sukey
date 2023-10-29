---
title: Transaction lifecycle
sidebar_position: 8
---

Transactions on Linea mainnet and testnet proceed through the following steps:

## Step 1: Submission

This is where every transaction originates: at the wallet level, a user signs a transaction and broadcasts it to Linea. From here, transactions head straight to the **mempool**, similarly to Ethereum, where they become pending transactions.

## Step 2: Block building

The Linea [sequencer](./sequencer) is responsible for ordering, building, and executing blocks. For each transaction added to the mempool, the sequencer checks its validity, rejecting transactions as necessary. Transaction validity conditions are specific to Linea, and differ slightly from those on other networks, including Ethereum.

:::info

A transaction is rejected if it:

- Originates from a non-existent account, or an account that is not funded
- Has a gas price below the minimum
- Has a gas limit above the maximum
- Has the same nonce as another transaction from the same account (in this case, the transaction with the higher gas fee is chosen, and the other rejected)
- Has a total `calldata` size greater than 25kb.

:::

The sequencer orders transactions according the priority fee paid for each, a method known as a priority gas auction. So, having passed the above checks, valid transactions are placed into blocks in the correct sequence and executed.

At this point, the transaction's lifecycle is more or less complete — at least from a user perspective. The block containing the transaction has been added to the "head" of the Linea blockchain—the most recent block—and a transaction receipt is returned to the user's wallet as confirmation.

However, the transaction and its associated data will continue to be processed in order to generate ZK proofs. Let's press on.

## Step 3: Transaction data sent to the state manager

Data about the transaction and the state of the network at its time of execution are recorded in **traces**, an output of part of the sequencer called the [traces generator](./sequencer/traces-generator.md).  

Traces are passed to the state manager block-by-block and then used to update the network state. Once state is up to date, you'll see the transaction reflected and confirmed in your wallet.

With the transaction executed and state updated, the transaction has reached **soft finality**: as far as the Linea chain is concerned—if considered in isolation—your transaction is complete. A L2 like Linea doesn't work in isolation, though, as we know; so there is more work to be done before true finality is reached.

## Step 4: Conflation

The transaction's block will then be subject to [conflation](./sequencer/conflation.md), which combines two or more blocks' transaction data into a single data set (batch) that forms part of the package of data passed on to Ethereum. Combining the transaction data of multiple blocks means that a single proof can be used to verify a large volume of transactions, minimizing the costs of submitting proofs to L1.

Occasionally, a batch may only consist of one block, with no conflation having taken place. This occurs when chain activity levels are particularly high, and the block size is larger than normal.

Amongst Ethereum's L2 networks, Linea is the only network that uses batch conflation.

## Step 5: Generating a ZK proof using transaction data

With the block that contains the transaction's trace data conflated into a batch with one or more others, the only remaining task on the checklist to achieve **_hard_ finality** is to use the transaction's data—as contained in its trace—to generate a proof.

When prompted by the [Coordinator](./coordinator), Linea's [prover](./trace-expansion-proving) will first **expand** the trace, preparing it for inclusion in the proof. Linea's prover employs a two-stage method for developing the proofs that eventually get passed to L1, first developing an **inner proof** and then an **outer proof**.

The inner proof uses a combination of tools, including Arcane and Vortex, to recursively reduce the proof size. For a more in-depth look at Linea's inner proof system, see [this article](https://mirror.xyz/lineaecosystem.eth/J8TohEE6CM3p3hkSkkL1vyngqipqc-tygXzgXO8ducw).

Next, the outer proof is generated using the Consensys-maintained library [`gnark`](https://docs.gnark.consensys.net/), compressing the proof size even further. The resulting proof is what's known as a zk-SNARK: the proofs that are eventually submitted to Ethereum.

Since the trace data of _every_ transaction in _every_ block feeds into producing the final proof, the single transaction we started with remains vital to Linea's function, well beyond the point at which it achieves soft finality in [step 3](#step-3-transaction-data-sent-to-the-state-manager).

## Step 6: Batch finalization

The final step in the process is to finalize the batch by submitting it to Ethereum mainnet, proving its computational integrity. Since the batch of conflated blocks is comprised of transactions, our transaction is involved in this process as well.

Let's break down the two elements submitted to L1:

- The proof, as explained [above](#step-5-generating-a-zk-proof-using-transaction-data), and;
- `calldata`, the object in which L2 transaction data is stored. The public availability of `calldata` means that anyone can use it to reconstruct Linea's state. You can then compare this reconstruction to the contents of the proof, and verify the latter. This is what happens when the Linea rollup contract on L1 calls the the Ethereum verifier contract using `calldata`, determining whether or not to accept the batch as valid.

You can view `calldata` in completed batches on L1 by heading to the [Linea L1 rollup contract](https://etherscan.io/address/0xd19d4b5d358258f05d7b411e21a1460d11b0876f) and finding a transaction whose method is labelled as "Finalize Blocks". Once there, scroll and expand the "More details" section, and then decode the input data.

You can also view finalized batches on Lineascan, [here](https://lineascan.build/batches).

Once the proof is verified and two epochs have passed, the transaction becomes immutable history, and reaches **hard finality**. Its lifecycle is complete.
