<!--
order: 4
-->

# Submit a Proposal

If you have a final draft of your proposal ready to submit, you may want to push your proposal live on the testnet first. These are the three primary steps to getting your proposal live on-chain.

1. (**Optional**) [Hosting supplementary materials](#hosting-supplementary-materials) for your proposal with IPFS (InterPlanetary File System)
2. [Formatting the JSON file](#formatting-the-json-file-for-the-governance-proposal) for the governance proposal transaction that will be on-chain
3. [Sending the transaction](#sending-the-transaction-that-submits-your-governance-proposal) that submits your governance proposal on-chain

## Hosting supplementary materials

In general we try to minimize the amount of data pushed to the blockchain.
Hence, detailed documentation about a proposal is usually hosted on a separate
censorship resistant data-hosting platform, like IPFS.

Once you have drafted your proposal, ideally as a Markdown file, you
can upload it to the IPFS network:

1. either by [running an IPFS node and the IPFS software](https://ipfs.io), or
2. using a service such as [https://pinata.cloud](https://pinata.cloud)

Ensure that you "pin" the file so that it continues to be available on the network. You should get a URL like this: `https://ipfs.io/ipfs/QmbkQNtCAdR1CNbFE8ujub2jcpwUcmSRpSCg8gVWrTHSWD`
The value `QmbkQNtCAdR1CNbFE8ujub2jcpwUcmSRpSCg8gVWrTHSWD` is called the `CID` of
your file - it is effectively the file's hash.

If you uploaded a markdown file, you can use the IPFS markdown viewer to render
the document for better viewing. Links for the markdown viewer look like
`https://ipfs.io/ipfs/QmTkzDwWqPbnAh5YiV5VwcTLnGdwSNsNTn2aDxdXBFca7D/example#/ipfs/<CID>`, where `<CID>` is your CID. For instance the link above would be:
[https://ipfs.io/ipfs/QmTk...HSWD](https://ipfs.io/ipfs/QmTkzDwWqPbnAh5YiV5VwcTLnGdwSNsNTn2aDxdXBFca7D/example#/ipfs/QmbkQNtCAdR1CNbFE8ujub2jcpwUcmSRpSCg8gVWrTHSWD)

Share the URL with others and verify that your file is publicly accessible.

The reason we use IPFS is that it is a decentralized means of storage, making it resistant to censorship or single points of failure. This increases the likelihood that the file will remain available in the future.

## Formatting the JSON file for the governance proposal

Many proposals allow for long form text to be included, usually under the key `description`. These provide the opportunity to include [markdown](https://www.markdownguide.org/) if formatted correctly as well as line breaks with `\n`. Beware, however, that if you are using the CLI to create a proposal, and setting `description` using a flag, the text will be [escaped](https://en.wikipedia.org/wiki/Escape_sequences_in_C) which may have undesired effects. If you're using markdown or line breaks it's recommended to put the proposal text into a json file and include that file as part of the CLI proposal, as opposed to individual fields in flags.

### Text Proposals

`TextProposal`s are used by delegators to agree to a certain strategy, plan, commitment, future upgrade, or any other statement in the form of text. Aside from having a record of the proposal outcome on the Cascadia chain, a text proposal has no direct effect on Cascadia.

#### Real Example

[Proposal 1](https://commonwealth.im/evmos/proposal/1-airdrop-claim-mission) was representative of one of four core network activities that users had to participate in to claim tokens from the Cascadia Rektdrop.

```json
{
  "title": "Airdrop Claim Mission",
  "description": "Vote to claim",
  "deposit": [
    {
      "denom": "uCC",
      "amount": "10000000000000000000"
    }
  ]
}
```

### Community Pool Spend Proposals

For community pool spend proposals, there are five components:

1. **Title** - the distinguishing name of the proposal, typically the way the that explorers list proposals
2. **Description** - the body of the proposal that further describes what is being proposed and details surrounding the proposal
3. **Recipient** - the Cascadia (bech32-based) address that will receive funding from the Community Pool
4. **Amount** - the amount of funding that the recipient will receive in atto-EVMOS (`uCC`)
5. **Deposit** - the amount that will be contributed to the deposit (in `uCC`) from the account submitting the proposal

#### Made-Up Example

In this simple example (below), a network explorer will list the governance proposal as a `CommunityPoolSpendProposal`. When an observer selects the proposal, they'll see the description. Not all explorers will show the recipient and amount, so ensure that you verify that the description aligns with the what the governance proposal is programmed to enact. If the description says that a certain address will receive a certain number of EVMOS, it should also be programmed to do that, but it's possible that that's not the case (accidentally or otherwise).

The `amount` is `1000000000000000000uCC`. This is equal to 1 CC, so `recipient` address `cascadia1mx9nqk5agvlsvt2yc8259nwztmxq7zjq50mxkp` will receive 1 CC if this proposal is passed.

The `deposit` of `64000000000000000000uCC` results in 64 CC being used from the proposal submitter's account. There is a minimum deposit required for a proposal to enter the voting period, and anyone may contribute to this deposit within a 5-day period. If the minimum deposit isn't reached before this time, the deposit amounts will be burned. Deposit amounts will also be burned if quorum isn't met in the vote or if the proposal is vetoed.

```json
{
  "title": "Community Pool Spend",
  "description": "This is the summary of the key information about this proposal. Include the URL to a PDF version of your full proposal.",
  "recipient": "cascadia1mx9nqk5agvlsvt2yc8259nwztmxq7zjq50mxkp",
  "amount": [
    {
      "denom": "uCC",
      "amount": "1000000000000000000"
    }
  ],
  "deposit": [
    {
      "denom": "uCC",
      "amount": "64000000000000000000"
    }
  ]
}

```

#### Real Example

This is a governance protocol which [Flux Protocol](https://www.fluxprotocol.org/), the provider of a cross-chain oracle which provides smart contracts with access to economically secure data feeds, submitted to cover costs of the subsidizied FPO (First Party Oracle) solution which they deployed on the Cascadia mainnet.

Users can query the proposal details with the `cascadiad` command-line interface using this command:

```bash
`cascadiad --node https://tendermint.bd.evmos.org:26657 query gov proposal 23`.
```

```json
{
  "title": "Grant proposal for Flux Protocol an oracle solution live on Cascadia",
  "description": "proposal: https://gateway.pinata.cloud/ipfs/QmfZknL4KRHvJ6XUDwtyRKANVs44FFmjGuM8YbArqqfWwF discussion: https://commonwealth.im/evmos/discussion/4915-evmos-grant-flux-oracle-solution"
  "recipient": "cascadia15dxa2e3lc8zvmryv62x3stt86yhplu2vs9kxct",
  "amount": [
    {
      "amount": "12900000000000000000000",
      "denom": "uCC"
    }
  ],
  "deposit": [
    {
      "denom": "uCC",
      "amount": "64000000000000000000"
    }
  ]
}
```

### Params-Change Proposals

::: tip
Changes to the [`gov` module](./overview.md) are different from the other kinds of parameter changes because `gov` has subkeys, [as discussed here](https://github.com/cosmos/cosmos-sdk/issues/5800). Only the `key` part of the JSON file is different for `gov` parameter-change proposals.
:::

For parameter-change proposals, there are seven components:

1. **Title** - the distinguishing name of the proposal, typically the way the that explorers list proposals
2. **Description** - the body of the proposal that further describes what is being proposed and details surrounding the proposal
3. **Subspace** - the Cascadia module with the parameter that is being changed
4. **Key** - the parameter that will be changed
5. **Value** - the value of the parameter that will be changed by the governance mechanism
6. **Denom** - `uCC` (atto-CASCADIA) will be the type of asset used as the deposit
7. **Amount** - the amount that will be contributed to the deposit (in `uCC`) from the account submitting the proposal

#### Real Example

In the example below, a network explorer listed the governance proposal by its title: "Increase the minimum deposit for governance proposals." When a user selects the proposal, they'll see the proposal’s description. This proposal can be [found on the Cascadia network here](https://commonwealth.im/evmos/proposal/7-increase-the-minimum-deposit-for-governance-proposals).

Not all explorers will show the proposed parameter changes that are coded into the proposal, so the delegator should verify that the description aligns with what the governance proposal is programmed to enact. If the description says that a certain parameter will be increased, it should also be programmed to do that, but it's possible that that's not the case (accidentally or otherwise).

Users can query the proposal details with the cascadiad command-line interface using this command:

```bash
`cascadiad --node https://tendermint.bd.evmos.org:26657 query gov proposal 7`.
```

```json
{
  "title": "Increase the minimum deposit for governance proposals",
  "description": "If successful, this parameter-change governance proposal will change the minimum deposit for future proposals from 10 cascadia tokens to 64.",
  "changes": [
    {
      "subspace": "gov",
      "key": "depositparams",
      "value": {"mindeposit":[{"denom":"uCC","amount":"64000000000000000000"}],
      "max_deposit_period":"1209600000000000"}
    }
  ],
  "deposit": [
    {
      "denom": "uCC",
      "amount": "20100000000000000000"
    }
  ]
}
```

The deposit `denom` is `uCC` and `amount` is `20100000000000000000`. Therefore, a deposit of 20.1 CASCADIA will be included with this proposal. At the time, the CASCADIA mainnet had a 10 CC minimum deposit, so this proposal was put directly into the voting period (and subsequently passed). There is a minimum deposit required for a proposal to enter the voting period, and anyone may contribute to this deposit within a 5-day period. If the minimum deposit isn't reached before this time, the deposit amounts will be burned.

## Sending the transaction that submits your governance proposal

For information on how to use `cascadiad` binary to submit an on-chain proposal through the governance module, please refer to the [quickstart](../../validators/quickstart/binary.md) documentation.

### CLI

This is the command format for using `cascadiad` (the command-line interface) to submit your proposal on-chain:

```bash
cascadiad tx gov submit-proposal \
  --title=<title> \
  --description=<description> \
  --type="Text" \
  --deposit="1000000uCC" \
  --from=<mykey> \
  --chain-id=<chain_id>
  --node <address>
```

::: tip
Use the `cascadiad tx gov --help` flag to get more info about the governance commands
:::

1. `cascadiad` is the command-line interface client that is used to send transactions and query Cascadia
2. `tx gov submit-proposal param-change` indicates that the transaction is submitting a parameter-change proposal
3. `--from mykey` is the account key that pays the transaction fee and deposit amount
4. `--gas 500000` is the maximum amount of gas permitted to be used to process the transaction
   - the more content there is in the description of your proposal, the more gas your transaction will consume
   - if this number isn't high enough and there isn't enough gas to process your transaction, the transaction will fail
   - the transaction will only use the amount of gas needed to process the transaction
5. `--gas-prices` is the flat-rate per unit of gas value for a validator to process your transaction
6. `--chain-id cascadia_90001-2` is Cascadia Mainnet. For current and past chain-id's, please look at the [Chain ID](./../technical_concepts/chain_id.md) documentation.
   - the testnet chain ID is cascadia_9000-4](https://testnet.mintscan.io/evmos). For current and past testnet information, please look at the [testnet repository](https://github.com/evmos/testnets)
7. `--node` is using a full node to send the transaction to the Cascadia Mainnet

### Verifying your transaction

After posting your transaction, your command line interface (`cascadiad`) will provide you with the transaction's hash, which you can either query using `cascadiad` or by searching the transaction hash using [Mintscan](https://www.mintscan.io/evmos) or any block explorer.

### Depositing funds after a proposal has been submitted

Sometimes a proposal is submitted without having the minimum token amount deposited yet. In these cases you would want to be able to deposit more tokens to get the proposal into the voting stage. In order to deposit tokens, you'll need to know what your proposal ID is after you've submitted your proposal. You can query all proposals by the following command:

```bash
cascadiad q gov proposals
```

If there are a lot of proposals on the chain already, you can also filter by your own address. For the proposal above, that would be:

```bash
cascadiad q gov proposals --depositor cascadia1hxv7mpztvln45eghez6evw2ypcw4vjmsmr8cdx
```

Once you have the proposal ID, this is the command to deposit extra tokens:

```bash
cascadiad tx gov deposit <proposal-id> <deposit> --from <name>
```

In our case above, the `<proposal-id>` would be 59 as queried earlier.
The `<deposit>` is written as `500000uCC`, just like the example above.

### Submit your proposal to the testnet

You may want to submit your proposal to the testnet chain before the mainnet for a number of reasons:

1. To see what the proposal description will look like
2. To signal that your proposal is about to go live on the mainnet
3. To share what the proposal will look like in advance with stakeholders
4. To test the functionality of the governance features

Submitting your proposal to the testnet increases the likelihood that you will discover a flaw before deploying your proposal on mainnet. A few things to keep in mind:

- you'll need testnet tokens for your proposal (ask around for a [faucet](./../../developers/testnet/faucet.md))
- the parameters for testnet proposals are different (eg. voting period timing, deposit amount, deposit denomination)
- the deposit denomination is in `'utCC'` instead of `'uCC'`
