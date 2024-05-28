# Tokenomics
The SWTCH DAO is responsible for the stewardship of the SWTCH Platform. It enables users and operators to manage how decentralized network services function and define the future of the SWTCH Platform.

## Token
The SWTCH token is the native token used as a reward mechanism within the SWTCH Protocol.

## Token Supply
The maximum supply of the platform is set at 11 billion (11B) SWTCH tokens, with initial rewards starting at 1,000,000 SWTCH per day, divided among the operators on the network.

#### Key Points:
- The SWTCH governance token can only be earned by operating verifiable services for the SWTCH protocol.
- SWTCH will deploy the governance token to multiple blockchains via the SWTCH Protocol.
- SWTCH is a governance token used to manage the network by voting on features and rewards for providing services on the network.

## Token Reward Distribution
Token distribution is managed by the following protocol:

#### Key Points:
- Max Supply: 11 billion tokens.
- Initial Daily Reward: Approximately 7,534,246 tokens distributed across all operators on the network.
- Halving Period: Every 2 years (730 days).
- Minting Mechanism: Ensures the daily reward is distributed, adjusted for halving, and does not exceed the max supply.

## Work Rewards
- Submit Work: Operators submit work to the protocol in the form of a Work Order for each service provided. Users can start a payment service with an operator in the form of Subscription, Payment Channel, Bi-Directional, or Escrow. The Work Order is verified by the SWTCH Protocol.
- Withdraw Fees: Operators can withdraw fees from the SWTCH DAO for the services rendered. These fees are collected by the SWTCH Protocol and held until the operator withdraws their earnings.
- Performance-Based Reward Distribution: If an operator completes only part of their assigned workloads (e.g., 8 out of 10), the operator is entitled to a proportionate share of the fees (80%). The remaining rewards (20%) are sent to the SWTCH DAO Treasury. This ensures that any remaining rewards from operators who provide subpar or intermittent services are redirected to the Treasury.

## WorkOrder Specification
A WorkOrder is a structured record that details the type of service provided, the workloads assigned and completed, and the operator's performance. It ensures transparency and accountability in the service operations and allows for proper fee distribution based on the completion rate.

#### WorkOrder Structure
- Service Type
  - Definition: The type of service being provided.
  - Values: Payment Channel, Subscription, Bi-Directional, Escrow. 
- Workloads Assigned
  - Definition: The total number of workloads assigned to the service operator.
  - Type: Integer.
- Workloads Completed
  - Definition: The total number of workloads completed by the service operator.
  - Type: Integer.
- Completion Rate
  - Definition: The ratio of completed workloads to assigned workloads, used to determine the percentage of fees the operator is entitled to.
  - Type: Float (calculated as Workloads Completed / Workloads Assigned).
- Fee Entitlement
  - Definition: The percentage of fees the operator is entitled to based on the completion rate.
  - Type: Float (same as Completion Rate).
- Fee Refund
  - Definition: The percentage of fees subject to refund based on incomplete workloads.
  - Type: Float (calculated as 1 - Completion Rate).
- Timestamp
  - Definition: The timestamp when the WorkOrder was created.
  - Type: DateTime.
- Signature
  - Definition: A digital signature of the operator, verifying the authenticity and accuracy of the WorkOrder.
  - Type: String (digital signature).

#### WorkOrder Submission
The final WorkOrder, after all workloads are completed, should be submitted to the protocol for fee distribution.

- Steps:
  1. Track Workloads: Each time a workload is completed, it is recorded.
  2. Create Final WorkOrder: After completing all workloads, the operator creates the final WorkOrder, summarizing the assigned and completed workloads.
  3. Sign the WorkOrder: The operator signs the WorkOrder with their private key.
  4. Submit WorkOrder: The signed WorkOrder is submitted to the protocol submit work function.

By following this WorkOrder specification, the SWTCH protocol ensures fair and transparent compensation for service operators based on their performance, promoting accountability and efficiency in service delivery.

## Governance
The SWTCH governance model empowers token holders to influence the direction and development of the SWTCH Platform. Key aspects of this governance model include voting weight, voting mechanics, proposal costs, and the implementation of approved proposals.

#### Voting Weight
- Token-Based Voting: Each SWTCH token held by a user provides a corresponding vote weight. The more tokens a user holds, the greater their influence on governance decisions.
- Proof of Funds: During the voting phase for a proposal, the protocol holds the tokens as proof of funds. These tokens cannot be transferred during the voting period.

#### Voting Mechanics
- Voting Phase: Tokens are locked during the voting phase to ensure they serve as proof of funds. Transferring tokens during this phase will result in a fine, and the proposal associated with the transferred tokens will be rejected.
- Fine for Transfer: To maintain the integrity of the voting process, any token transfer during the voting phase incurs a fine, and the related proposal is automatically disqualified.

#### Proposal Submission
- Cost of Proposals: Submitting a proposal requires a specific token value, also held as proof of funds by the protocol. This cost ensures that only serious and well-considered proposals are submitted.
- Proof of Funds: The required token value for submitting a proposal must be provided as proof of funds, ensuring that the proposer has a stake in the outcome.

#### Implementation of Approved Proposals
- Post-Approval: Once a proposal is approved, the relevant protocol or platform component is upgraded in accordance with the approved changes. This ensures that the SWTCH Platform evolves in line with the collective decisions of its token holders.

By enabling token-based voting, implementing a proof of funds mechanism, and establishing a clear process for proposal submission and implementation, the SWTCH governance model ensures a transparent, fair, and efficient decision-making process.