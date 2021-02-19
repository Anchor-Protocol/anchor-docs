# Anchor Governance

{% hint style="info" %}
It is recommended to first start a community discussion at the Anchor Protocol Forum before submitting a poll.
{% endhint %}

Development and maturization of Anchor Protocol is driven by the Anchor community.

## Anchor Governance Token \(ANC\)

Governance over Anchor is managed by ANC token holders





## Polls

New governance proposals in Anchor are called **polls.** Any user can create a poll by paying an initial deposit of ANC tokens. If the poll fails to pass the minimum voting quorum, the ANC deposit is given to ANC stakers and distributed proportionately according to their relative stake.

Polls consist of a text description of the proposition \(with an optional URL to further resources / discussions\), and includes a list of executable messages that encode the instructions to be run if it passes. The message will be executed with the privileges of the [Anchor Gov Contract](../../smart-contracts/anchor-token/gov.md), which has the power to invoke any function defined by the other Anchor smart contracts.

Once submitted, a poll can be voted on by the community until its voting period has concluded. If the poll passes quorum and threshold conditions \(defined below\), it is ratified and its contents can automatically be applied after a set period of time. These changes take effect without requiring updates to the core Anchor Protocol contracts.

{% hint style="danger" %}
Staked ANC tokens utilized in on-going polls cannot be withdrawn until the poll completes. In addition, the number of ANC used in a proposal **cannot** be modified after the vote has been submitted.
{% endhint %}

## Poll Lifecycle

Governance polls in Anchor follow the below procedure:

1. A new poll is created with an initial ANC deposit of `proposal_deposit`
2. The poll enters the voting phase, where it can voted for by anybody with a staked ANC position. Users can vote `yes` or `no`, and can assign how many of their staked ANC to use for voting.
3. snapshot
4. The voting period ends after `voting_period` has passed.
5. The poll's votes a tallied. The poll passes if both quorum \(minimum participation of all staked ANC\) and threshold \(minimum ratio of `yes` to `no` votes\) are met.
6. If the poll passes, its contents can be executed after `effective_delay` blocks have passed. The poll must be executed prior to the `expiration_period`, otherwise it will automatically expire and no longer be considered valid.
