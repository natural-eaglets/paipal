# 🏛️ Vault Deployment Flow

[![Version](https://img.shields.io/badge/Version-v0.2-blue)](https://paipal.ai)
[![Security](https://img.shields.io/badge/Security-TEE_Protected-green)](https://paipal.ai)
[![Type](https://img.shields.io/badge/Type-Technical_Spec-orange)](https://docs.paipal.ai)

**AI-to-AI Coordination for Secure, Automated DeFi**

---

## System Flow Diagram

```mermaid
graph TB
    subgraph "1. User Request"
        A[User] -->|Submits Request| B[Paipal Interface]
        B -->|Specifies Parameters| C[Request Parameters]
        C -->|Contains| D[Supported Assets]
        C -->|Contains| E[Risk Level]
        C -->|Contains| F[Optional Filters]
    end

    subgraph "2. TEE-Protected AI Deployer"
        G[Deployer Agent] -->|Verifies| H[Request Validation]
        H -->|Generates| I[Smart Vault Contract]
        I -->|Applies| J[User Rules & Permissions]
    end

    subgraph "3. Ownership & Registration"
        K[Vault Ownership] -->|Assigned to| L[User On-Chain]
        M[Paipal Registry] -->|Registers| N[Vault Metadata]
    end

    subgraph "4. AI Strategy Agent"
        O[Strategy Agent] -->|Monitors| P[Balance & Markets]
        P -->|Generates| Q[Allocation Strategy]
        Q -->|Signs| R[Transaction Proposal]
    end

    subgraph "5. Executor Nodes"
        S[Executor Agents] -->|Execute| T[Signed Instructions]
        T -->|Perform| U[Fund Transfers]
        T -->|Perform| V[Bridge Operations]
        T -->|Deploy to| W[DeFi Strategies]
    end

    B --> G
    J --> K
    N --> O
    R --> S
```

## Detailed Process Flow

### 1. User Requests Vault Creation
A user submits a vault creation request via the Paipal interface or smart contract.

They specify:
* Supported assets (e.g. USDC, ETH)
* Risk level (e.g., low, medium, degen)
* Optional filters or constraints

### 2. TEE-Protected AI Deployer Instantiates the Vault
The request is handled by the Deployer Agent, an AI running inside a Trusted Execution Environment (TEE).

This agent:
* Verifies the request
* Generates the smart vault contract
* Applies user-defined rules and permissions

Permissions are minimal and trustless:
* Only the user can trigger withdrawals
* Only verified Executor Agents can move funds into strategies (never to arbitrary wallets)
* Cross-chain transfers are permissioned via secure channels (e.g., AggLayer + TEE signatures)

### 3. Ownership Assignment & Vault Registration
Vault ownership is assigned to the user on-chain.

The vault is registered in the Paipal Registry, allowing the AI Strategy Agent to start monitoring.

Metadata (risk score, preferences) is stored off-chain or in encrypted TEE memory.

### 4. AI Strategy Agent Kicks In
The AI Strategy Agent (also inside a TEE) starts:
* Monitoring the vault's balance and market conditions
* Generating allocation strategies based on risk preferences

Once it finds an optimal plan, it:
* Signs the proposed transaction
* Publishes it on-chain or to a secure message bus for Executor Agents

### 5. Executor Nodes Perform the Allocation
Executor Agents (community-run nodes) pick up signed instructions.

They cannot change or interpret logic — only execute:
* Transfer funds
* Bridge if needed
* Deploy to strategy (e.g. Aave, GMX, Curve)

Execution is trustless and verifiable:
* Instruction includes AI signature
* Executors are rewarded only if the action matches the AI's signed request

## Trust Model

| Component | Actor Type | TEE-Protected | Role |
|-----------|------------|---------------|------|
| Deployer Agent | AI | ✅ | Secure vault instantiation |
| Strategy Agent | AI | ✅ | Risk analysis + proposal |
| Executor Agent | Human Node | ❌ (verifies TEE signature) | Performs allocation |

## Security Considerations
* All parameters are validated before deployment
* Risk levels are strictly enforced
* Asset support is verified against protocol whitelist
* Optional filters undergo security checks

## Technical Implementation
The deployment process uses a secure, multi-step verification system to ensure:
* Parameter validation
* Security checks
* Proper contract initialization
* Correct asset configuration

## Notes
* All deployments are tracked and monitored
* Changes to vault parameters require governance approval
* Risk levels cannot be modified after deployment without formal process 