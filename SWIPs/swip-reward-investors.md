---
SWIP: <to be assigned>
title: Reward Investors
author: Joshua Pritikin (@jpritikin)
discussions-to: https://discord.com/channels/799027393297514537/808329804268699678
status: Draft
type: Core
created: 2024-03-02
---

<!--You can leave these HTML comments in your merged SWIP and delete the visible duplicate text guides, they will not appear and may be helpful to refer to if you edit it again. This is the suggested template for new SWIPs. Note that a SWIP number will be assigned by an editor. When opening a pull request to submit your SWIP, please use an abbreviated title in the filename, `SWIP-draft_title_abbrev.md`. The title should be 44 characters or less.-->

## Simple Summary
<!--"If you can't explain it simply, you don't understand it well enough." Provide a simplified and layman-accessible explanation of the SWIP.-->
Make the BZZ token appreciate in value to reward early investors.

## Abstract
<!--A short (~200 word) description of the technical issue being addressed.-->
A short (~200 word) description of the technical issue being addressed.

## Motivation
<!--The motivation is critical for SWIPs that want to change the Swarm protocol. It should clearly explain why the existing protocol specification is inadequate to address the problem that the SWIP solves. SWIP submissions without sufficient motivation may be rejected outright.-->
Under the current rules ([BZZ Tokenomics, Oct 25, 2021](https://medium.com/ethereum-swarm/swarm-tokenomics-91254cd5adf)), the BZZ token may not appreciate much in value. Some activities require purchasing BZZ:
- New node operators need to make a minimum deposit.
- Data archivists need to buy BZZ to purchase postage stamps.
- Data users need to buy miniscule amounts of BZZ to [pay for bandwidth](https://blog.ethswarm.org/foundation/2021/understanding-swarms-bandwidth-incentives/).

Other parties are natural sellers of BZZ:
- Node operators sell BZZ to pay for electricity and hardware.
- [Investors](https://medium.com/ethereum-swarm/swarm-tokenomics-91254cd5adf) may like to cash out their investment.

Swarm had historically ignored the economics of BZZ because it was more important to get the technology to work. Now that the technology is on the cusp of working, we have an opportunity to revamp the economics.

One purpose of the BZZ token was to [reward early adopters](https://blog.ethswarm.org/foundation/2021/swarm-is-airdropping-1000000-bzz/). By this metric, BZZ is a failure. Sold to the public for 1.92 DAI on June 2021, the token is worth about 0.5 DAI as of Apr 2024. Early contributors and investors deserve a return on investment. If the price of BZZ could be better linked to network usage then this would benefit practically all project participants.

It's not an apples-to-apples comparison, but Filecoin has a market cap of about 4.5 billion while BZZ is only 34 million (Mar 2024). That's a difference of more than two orders of magnitude.

## Specification
<!--The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for the current Swarm platform and future client implementations.-->

Send 10% of the BZZ stamp payment to the Swarm Foundation owned Uniswap v2 BZZ pool before sending the remainder to the redistribution lottery.

Eventually the node operator minimum deposit will need to be reduced to compensate for the increase in the value of BZZ. Note: A change is already on the roadmap planned to be implemented soon. This is to mitigate the risk of node operators against being overstaked. Since swarm stake is effectively a non-redeemable participation fee, if the price of bzz goes up substantially, then operators end up unreasonably high stakes. The solution is to have an effective or committed stake which is denominated in the unit price of storage. Whatever your staked bzz is worth above the committed stake, you can freely redeem at any point. This way, there is no disincentive to stake even if you correctly anticipate deflation. Without this, we would punish the pioneer operators. This feature is in the book, but for some reason not implemented, but will be cos it eliminates an unfortunate misalignment of incentives. (Viktor Tron, 06 Mar 2024 #bzz-tokenomics)

## Rationale
<!--The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.-->

The choice of 10% going to investors is arbitrary. The community can decide on the most suitable burn rate. As [a rule of thumb](https://www.brex.com/journal/what-is-a-good-profit-margin), 5% is a low margin, 10% is a healthy margin, and 20% is a high margin. The amount of stamp payments should be roughly equal to the [Network Total Monthly Rewards](https://blog.ethswarm.org/foundation/2024/state-of-the-network-january/).

Stamp payments are a great place to add this fee. Stamp buyers are expressing an intention to store data on the network which is the behavior that is synonymous with the success of the network. BZZ should be backed by future stamp fees.

To ensures that BZZ retains on-chain liquidity, the fee on stamp payments will be deposited into to a Swarm Foundation owned Uniswap v2 BZZ pool. This concept has already been proven by [Maker's smart burn engine](https://makerburn.com/#/buyback). See [here](https://vote.makerdao.com/polling/QmQmxEZp#poll-detail) for more background. Maker chose Uniswap v2 instead of more modern solutions because it is battle tested and has less attack surface. Uniswap v2 is one of the simplest way to provide market depth to a free floating token pair.

The Uniswap pool fee should not be so low as to encourage speculation (e.g., 0.01%). Excessive BZZ price volatility is not in the interest of actual users of the Swarm network. Therefore, the Uniswap pool fee should be fairly high, like 0.5%, to increase transaction costs a bit and discourage short-term speculation.

Prior to this SWIP, rational investors were only distantly concerned with the success of the network. The day-to-day sale of postage stamps was of little concern. After this SWIP, rational investors should want to optimize the price fluctuations of BZZ to encourage the purchase of postage stamps. Postage stamp purchasers are mostly concerned about buying storage and don't want to think about BZZ price. Therefore, rational investors will want to minimize BZZ price volatility.

For example, rational investors may want to allocate capital to a UniV3 LP pool with low fees (e.g., 0.01%) and a narrow price range. The price range would be updated at most weekly. This would allow small orders to get filled with very low fees as long as the trade happens within the narrow price range. This could help limit BZZ volatility.

### Background on the Price Oracle

> [Our price oracle](https://github.com/ethersphere/storage-incentives/blob/4fdb26e135f7391379ca84100b723f34a1a4175e/src/PriceOracle.sol#L93) is a simple thermostat model. It regulates the price down if there are signals of oversupply (of storage) and regulates it up if there is a signal of undersupply. A&A of Shtuka criticised this model that it also needs to regulate against fluctuations in price of BZZ. I argued that this is no more of a problem, that your real thermostate regulating temperature according to your comfort level given the outside temperature. However, the ultimate thermostate experience depends on the reactivity - if the coldest  winter day changes to the hottest summer day in a matter of seconds then your comfort might be too often not met cause the time it takes for the heating to adapt. (Viktor Tron, 04 Mar 2024 #bzz-tokenomics)

### Legal Analysis

Disclaimer: *The information contained in this section is intended for informational purposes only and does not constitute legal advice. The author is not a licensed attorney and cannot offer legal guidance or representation. Readers should not rely on this information to make decisions regarding any legal matter. It is strongly recommended that you consult with a qualified attorney in your jurisdiction for personalized legal advice specific to your situation. The author does not assume any responsibility for any actions taken or not taken based on the content of this section. The information presented may not be up-to-date or applicable to every situation.*

[The Howey test](https://www.investopedia.com/does-crypto-pass-the-howey-test-8385183) established criteria for whether a transaction qualifies as an investment contract. As of the June 2021 crowdsale, it could be anticipated that BZZ would not appreciate much in value. Developers owned at least 20% of the initial issuance and planned to redeem for DAI to fund development. Node deposits could be anticipated to reduce the supply of BZZ, but this would only matter if the network grew very large. Even then, the deposit amount will need to be reduced to avoid creating too high a barrier for new node operators. Therefore, BZZ seems to fail the Howey criterion of *expectation of profits*.

This SWIP proposes to change the economic properties of the BZZ token. Supported by revenue coming in from stamp fees, BZZ is expected to appreciate in value. However, the success of BZZ is no longer in the hands of a small team of developers. The Howey criterion of *reliance on the efforts of others* no longer applies. Even more so than Bitcoin, very many holders of BZZ are responsible for the success of the network. Software developers, node operators, postage stamp purchasers, and bandwidth users contribute value.

## Backwards Compatibility
<!--All SWIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The SWIP must explain how the author proposes to deal with these incompatibilities. SWIP submissions without a sufficient backwards compatibility treatise may be rejected outright.-->

## Test Cases
<!--Test cases for an implementation are mandatory for SWIPs that are affecting changes to data and message formats. Other SWIPs can choose to include links to test cases if applicable.-->
*Pending*

## Implementation
<!--The implementations must be completed before any SWIP is given status "Final", but it need not be completed before the SWIP is accepted. While there is merit to the approach of reaching consensus on the specification and rationale before writing code, the principle of "rough consensus and running code" is still useful when it comes to resolving many discussions of API details.-->
*Pending*

### Future

Instead of sending fees to a UniV2 LP position, it might be more advantageous to provide market depth using [Gelato's UniV3](https://medium.com/gelato-network/introducing-g-uni-lp-like-a-pro-in-uniswap-v3-8fd6fdf9fc35) automation tools. Using these tools, the liquidity could be deployed in a narrow range with a very low fee (0.01%). Some customization of the [code](https://github.com/gelatodigital/g-uni-v1-core) would be needed to ensure that the price range can only be updated slowly (weekly) and to prevent large price jumps (limit price changes to 1% or 5% per week).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
