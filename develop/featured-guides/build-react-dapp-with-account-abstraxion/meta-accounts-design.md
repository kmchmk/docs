# Meta Accounts Design

**Overview**

XION’s modular _meta accounts_ introduce a highly adaptable and secure account creation & management framework.

Built at the protocol-level, _meta accounts_ have a host of benefits over traditional crypto wallets and enable novel Web3 application use-cases. See [generalized-abstraction](../../../learn/learn-about-xion/generalized-abstraction/ "mention") for more information around use cases and features.

## Components

<details>

<summary>Identity Provider</summary>

In the case of social login a trusted Identity provider must be used to handle the confirmation of identity, in this case [Stytch](https://stytch.com/) is being used.

</details>

<details>

<summary>Account API</summary>

A set of services used to assist and sponsor new meta account creation.&#x20;

See the [repo](https://github.com/burnt-labs/account-abstraction-api)

</details>

<details>

<summary>Abstraxion Library</summary>

This graz-based frontend library aids in integration with your react front end.

See the [repo](https://github.com/burnt-labs/abstraxion)

</details>

<details>

<summary>Meta Account Smart Contract</summary>

This contract along with a custom XION module represent the core of the meta account functionality.

See the [repo](https://github.com/burnt-labs/contracts)

</details>

## Supported Authentication Methods

* Email login
* (MetaMask, biometrics, and many more coming soon)

## Workflows



```mermaid
sequenceDiagram
    Actor User
    participant DAPP
    participant AbstraxionLibrary as Abstraxion Library
    participant AccountManagementDashboard as Account Management Dashboard
    participant XionChain as Xion Chain
    User->>DAPP: Load Dapp
    User->>DAPP: Clicks 'Mint NFT'
    DAPP->>AbstraxionLibrary: connect()
    AbstraxionLibrary->>AbstraxionLibrary: Check for Dapp SessionKey 
    AbstraxionLibrary->>AbstraxionLibrary: Check Dapp SessionKey Authz Expiry
    Note right of AbstraxionLibrary: Note: Authz grants are a "session"
    alt no active Dapp SessionKey
        AbstraxionLibrary->>AbstraxionLibrary: Generate one time private SessionKey
        AbstraxionLibrary->>AccountManagementDashboard: Redirect user along with permission scope 
        AccountManagementDashboard->>AccountManagementDashboard: Check for an active connection
        alt no active connection
            AccountManagementDashboard->>User: Initiate and complete connect flow
        end
        AccountManagementDashboard->>User: Show authz permission dialog
    end
    User->>AccountManagementDashboard: Accept permissions
    AccountManagementDashboard->>XionChain: Submit transaction with authz grants
    alt transaction successful
        XionChain->>DAPP: Notify transaction success
        DAPP->>User: Return user to dapp
    else transaction failed
        XionChain->>AccountManagementDashboard: Notify transaction failure
        AccountManagementDashboard->>User: Show error modal
    end
    User->>DAPP: Clicks 'Mint NFT'
    DAPP->>AbstraxionLibrary: Locally sourced transaction
    AbstraxionLibrary->>AbstraxionLibrary: Autosigned using Dapp SessionKey 
    AbstraxionLibrary->>XionChain: Submitted 
```



