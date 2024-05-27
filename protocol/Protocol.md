# SWTCH Protocol Specification

## 1. Introduction
#### 1.1 Purpose
This specification describes the design and implementation details of the SWTCH Protocol, an upgradeable protocol that integrates various components for managing identities, networks, secrets, payments, and tokens. The protocol is intended to evolve over time and transition governance from an admin to a Decentralized Autonomous Organization (DAO).

#### 1.2 Scope
The document covers the following areas:
- Overview of the SWTCH Protocol
- Detailed descriptions of each component managed by the protocol
- Core responsibilities of the protocol
- Unit tests for verifying the functionality and integrity of the protocol

#### 1.3 Audience
This specification is intended for developers, architects, and other stakeholders involved in the development and maintenance of the SWTCH Protocol.


## 2. SWTCH Protocol Overview

#### 2.1 Upgradeable Protocol
The protocol is designed to be upgradeable, allowing for seamless updates and improvements without disrupting existing functionality.

#### 2.2 Governance Structure
Initially managed by an Admin, governance will transition to the SWTCH Decentralized Autonomous Organization (DAO) to ensure decentralized decision-making and administration.

## 3. Components Managed by the Protocol
#### 3.1 Identity Manager
- Purpose: Handles the creation, registration, and management of identities through various identity providers.
- Common Interface: Defines a standard interface for all identity providers.
```solidity
// SPDX-License-Identifier: GPL-3
pragma solidity ^0.8.24;

/**
 * @title IIdentityProvider
 * @notice Identity interface for all IdentityProvider implementations.
 */
interface IIdentityProvider {
    function isValidIdentity(address user) external view returns (bool);
    function getIdentityDetails(address user) external view returns (bytes memory);
}
```
- Example Implementation: Combining ERC-725X and ERC-725Y.
```Solidity
// SPDX-License-Identifier: GPL-3
pragma solidity ^0.8.24;

import "./ERC725X.sol";
import "./ERC725Y.sol";

/**
 * @title Identity
 * @notice Combined identity provision using ERC-725X and ERC-725Y. Allows a single contract to manage identities and their claims.
 */
contract ERC725Identity is ERC725X {
    constructor(address _owner) ERC725X(_owner) {}
}
```
- Functions:
  - addIdentityProvider(address provider): Adds a new identity provider.
  - removeIdentityProvider(address provider): Removes an identity provider.
  - isValidIdentity(address user): Checks if a user has a valid identity through any registered provider.
  - getIdentityDetails(address user): Retrieves the identity details of a user from the appropriate provider.

- Events:
  - IdentityProviderAdded(address indexed provider)
  - IdentityProviderRemoved(address indexed provider)
  - IdentityValidated(address indexed user, bool isValid)
  - IdentityDetailsFetched(address indexed user, bytes details)

#### 3.2 Network Manager
- Purpose: Manages network services, including creation, registration, and updates.
- Functions:
  - createNetworkService(address owner, string memory serviceName): Creates a new network service.
    - Requirements: Each network service creation requires an Identity or address registered with one of the registered identity providers, such as Native ERC-725, Civic, or Polygon ID.
  - registerNetworkService(uint256 serviceId): Registers an existing network service.
  - updateNetworkService(uint256 serviceId, string memory newServiceName): Updates details of an existing network service.
  - deleteNetworkService(uint256 serviceId): Deletes a network service.
- Attributes:
  - Trusted nodes, whitelist/blocklist, connection protocols, public/private status, sub-networks, and pricing information.
- Events:
  - NetworkServiceCreated(uint256 indexed serviceId, address indexed owner, string serviceName)
  - NetworkServiceRegistered(uint256 indexed serviceId)
  - NetworkServiceUpdated(uint256 indexed serviceId, string newServiceName)
  - NetworkServiceDeleted(uint256 indexed serviceId)

#### 3.3 Secrets Manager
- Purpose: Responsible for creating, registering, and managing secrets.
- Functions:
  - createSecret(address owner, string memory secretData): Creates a new secret.
    - Requirements: Each secret service creation requires an Identity or address registered with one of the registered identity providers, such as Native ERC-725, Civic, or Polygon ID.
  - registerSecret(uint256 secretId): Registers an existing secret.
  - updateSecret(uint256 secretId, string memory newSecretData): Updates details of an existing secret.
  - deleteSecret(uint256 secretId): Deletes a secret.
- Security: Provides secure storage and access to sensitive information.
- Events:
  - SecretCreated(uint256 indexed secretId, address indexed owner, string secretData)
  - SecretRegistered(uint256 indexed secretId)
  - SecretUpdated(uint256 indexed secretId, string newSecretData)
  - SecretDeleted(uint256 indexed secretId)

#### 3.4 Payments Manager
- Purpose: Manages payment channels, escrow, proof of funds, and subscriptions.
- Functions:
  - createPaymentService(address owner, string memory serviceType, address user, uint256 amount): Creates a new payment service.
    - Requirements: 
      - Each payment service creation requires an Identity or address registered with one of the registered identity providers, such as Native ERC-725, Civic, or Polygon ID.
      - The payment service must be one of the following available financial services:
        - Payment Channel (one-way)
        - Bi-directional Payment Channel
        - Escrow (Native Crypto, ERC-20, ERC-721)
        - Proof of Funds (Native Crypto, ERC-20, ERC-721)
        - Subscriptions (Native Crypto, ERC-20, ERC-721)
  - registerPaymentChannel(uint256 channelId): Registers an existing payment channel.
  - updatePaymentChannel(uint256 channelId, uint256 newAmount): Updates details of an existing payment channel.
  - deletePaymentChannel(uint256 channelId): Deletes a payment channel.
- Events:
  - PaymentServiceCreated(uint256 indexed channelId, address indexed owner, string serviceType, address user, uint256 amount)
  - PaymentChannelRegistered(uint256 indexed channelId)
  - PaymentChannelUpdated(uint256 indexed channelId, uint256 newAmount)
  - PaymentChannelDeleted(uint256 indexed channelId)

#### 3.5 Tokens Manager
- Purpose: Handles the creation, deployment, and management of various token standards (ERC-20, ERC-721, ERC-1155).
- Functions:
  - createToken(string memory name, string memory symbol, uint8 decimals, uint256 initialSupply): Creates a new ERC-20 token.
    - Requirements: Each token service creation requires an Identity or address registered with one of the registered identity providers, such as Native ERC-725, Civic, or Polygon ID.
  - registerToken(uint256 tokenId): Registers an existing token.
  - updateToken(uint256 tokenId, string memory newName, string memory newSymbol): Updates details of an existing token.
  - deleteToken(uint256 tokenId): Deletes a token.
- Standard Calls: Provides standard calls for each token type.
- Events:
  - TokenCreated(uint256 indexed tokenId, string name, string symbol, uint8 decimals, uint256 initialSupply)
  - TokenRegistered(uint256 indexed tokenId)
  - TokenUpdated(uint256 indexed tokenId, string newName, string newSymbol)
  - TokenDeleted(uint256 indexed tokenId)

## 4. Core Protocol Responsibilities

#### 4.1 Submit Work
- Purpose: Allows identities to submit transactions that include payment channel transactions.
- Function: Ensures work submissions are properly recorded and associated with the correct identities.
- Functions:
  - submitWork(uint256 identityId, string memory workDetails): Submits work for a specific identity.
  - Events:
    - WorkSubmitted(uint256 indexed identityId, string workDetails)

#### 4.2 Withdraw Rewards
- Purpose: Enables users to withdraw earned rewards using an ERC-20 network token.
- Function: Ensures the secure and accurate distribution of rewards based on contributions.
- Functions:
  - withdrawRewards(uint256 identityId, uint256 amount): Withdraws rewards for a specific identity.
- Events:
  - RewardsWithdrawn(uint256 indexed identityId, uint256 amount)

#### 4.3 Withdraw Fees
- Purpose: Facilitates the withdrawal of fees earned from providing data or services.
- Function: Ensures proper accounting and distribution of earned fees.
- Functions:
  - withdrawFees(uint256 identityId, uint256 amount): Withdraws fees for a specific identity.
- Events:
  - FeesWithdrawn(uint256 indexed identityId, uint256 amount)

## 5. Unit Tests

#### 5.1 Upgradeable Protocol
- test_protocol_initialization(): Verifies the correct initialization of the protocol.
- test_protocol_upgrade(): Verifies the upgrade functionality of the protocol.

#### 5.2 Governance Structure
- test_admin_management(): Tests the management functions for the admin.
- test_dao_ownership_transition(): Tests the transition of ownership to the DAO.

#### 5.3 Protocol Responsibilities
- test_submit_work(): Verifies the submission of work transactions.
- test_withdraw_rewards(): Verifies the withdrawal of rewards.
- test_withdraw_fees(): Verifies the withdrawal of fees.

## 6. Implementation Guidelines
#### 6.1 Solidity
- Contracts: Define each component as a separate contract with the necessary functions and events.
- Upgradeability: Use a proxy pattern or the OpenZeppelin Upgradeable Contracts library.
- Security: Ensure proper access controls and validations.

#### 6.2 Rust
- Modules: Define each component as a module with associated functions and data structures.
- Testing: Use Rust's testing framework to implement unit tests.
- Integration: Ensure compatibility with smart contract platforms like Ethereum through libraries such as ethers or web3.
