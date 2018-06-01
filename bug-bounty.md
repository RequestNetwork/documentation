---
description: >-
  Security is a top priority for Request Network. To make sure everything stays
  secure we have a Bug Bounty.
---

# Bug bounty

![](.gitbook/assets/bugbounty.png)

The Bug Bounty currently targets the smart contracts listed below. The branch to audit is **master.**

* contracts/core/Administrable.sol
* contracts/core/Burner.sol
* contracts/core/RequestCore.sol
* contracts/core/RequestCollectInterface.sol
* contracts/core/RequestCurrencyContractInterface.sol
* contracts/synchrone/RequestEthereumCollect.sol
* contracts/synchrone/RequestEthereum.sol
* contracts/synchrone/RequestERC20.sol
* contracts/asynchrone/RequestBitcoinNodesValidation.sol
* contracts/base/math/SafeMathInt.sol
* contracts/base/math/SafeMathUint8.sol
* contracts/base/math/SafeMathUint96.sol

All Smart Contracts can be found [in their dedicated folder on Github](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/requestNetworkSmartContracts).

**Critical vulnerabilities will be rewarded up to $20 000, while major bugs will be rewarded up to $15 000.**

### Rules & Rewards

The rules of our bug bounty program are the same that apply to the Ethereum protocol: [https://bounty.ethereum.org](https://bounty.ethereum.org/)

* Issues that have already been submitted by another user or are already known to the Request Network team are not eligible for bounty rewards.
* Public disclosure of a vulnerability makes it ineligible for a bounty.
* The Request Network core development team, employees, and all other people paid by the Request Network Foundation, directly or indirectly, are not eligible for rewards.
* The Request Network bounty program considers a number of variables in determining rewards. Determinations of eligibility, score and all terms related to rewards are at the sole and final discretion of the Request Network Foundation bug bounty panel.

The value of rewards paid out will vary depending on severity. The severity is calculated according to the OWASP risk rating model based on Impact and Likelihood:

![](.gitbook/assets/severity.png)

Reward sizes are guided by the rules below, but are ultimately determined at the sole discretion of the Request Network Foundation bug bounty panel.

* Critical: up to $20 000
* High: up to $15 000
* Medium: up to $10 000
* Low: up to $2 000
* Note: up to $500

All bounty will be paid in **ether \(ETH\).**

The bug bounty program has no end date until communicated otherwise. We encourage you to report the bugs as an issue [on the Github repository](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/requestNetworkSmartContracts). You can also email [security@request.network](mailto:security@request.network). Anonymous submissions are welcome.

