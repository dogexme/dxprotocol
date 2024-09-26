
# DX Protocol Overview

**DX Protocol** is not just a protocol; it’s a fully extensible component system based on the Dogecoin blockchain. Essentially, Satoshi Nakamoto defined a rule set that generates UTXO data and records it on a blockchain. The default consensus is a full address indexing system according to UTXO rules, which tracks balances by address. Similarly, anyone can record rule-based data on-chain and freely define any set of rules, as long as the data is transparent and the indexing system is appropriately established.

source code https://github.com/dxteam2024/picador

## Protocol Components

The protocol defines two primary components:
1. **Token**  
2. **Swap**

All components share the same data and You can start from any block, and the service provider can define it freely.It won’t affect the ledger, it only impacts the starting point of the data you want to retrieve. Components are uniquely identified by `Dx+${txid}`. Component functions use the first input of a transaction as the executor and the first `p2shinput` containing protocol data as the input parameter for the function. Protocol data begins with the `DX` prefix. 

For example, deploying a token follows the format:  
`DX T D test 10000`

### UTXO Management

According to the protocol, UTXOs smaller than 0.1 DOGE are invalid transactions. Therefore, no dust will be created within this protocol. Since the input address is used as the accounting unit, the collection of UTXOs becomes irrelevant, so it won't make dogecoin's utxo data expand

## Parameter Rules and Descriptions

### 1. Token Deploy
```
DX T D <symbol> <supply> <0/1>
```
- **DX**: Protocol prefix  
- **T**: Token component code  
- **D**: Deploy  
- **symbol**: The token symbol (3-15 characters in length)  
- **supply**: Initial supply (up to `1e8`, integer-only at deployment)  
- **0/1**: Whether additional supply can be added (`0`: no, `1`: yes)  

**Example**:  
`DX T D test 10000 1` — This deploys a token with symbol "test", an initial supply of 10,000, and allows supply expansion.

### 2. Token Transfer
```
DX T T <dxid> <amount1> <amount2> <amount3>
```
- **T**: Transfer  
- **dxid**: Unique token identifier  
- **amount1 amount2 amount3**: Amounts to transfer to multiple addresses, where each output corresponds to a non-sender address.  

**Example**:  
`DX T T 1234abc 10000.90 200 3000` — Transfers 10,000.90, 200, and 3,000 tokens to three different addresses, excluding the sender.

### 3. Token Print (Only deployer address)
```
DX T P <dxid> <amount>
```
- **P**: Print  
- **dxid**: Unique token identifier  
- **amount**: The number of new tokens to be minted (integer, up to `1e8`).

**Example**:  
`DX T P 1234abc 10000` — Mints 10,000 new tokens.

### 4. Token Burn (Only deployer address)
```
DX T B <dxid> <amount>
```
- **B**: Burn  
- **dxid**: Unique token identifier  
- **amount**: The number of tokens to burn (must be a positive integer).

**Example**:  
`DX T B 1234abc 10000` — Burns 10,000 tokens.

## Swap Component

### 1. Swap Deploy
```
DX T_S1 D <dxid1> <dxid2>
```
- **T_S1**: Swap component  
- **D**: Deploy  
- **dxid1** and **dxid2**: Identifiers for the two tokens being swapped.

### 2. Swap Mint (Add Liquidity)
```
DX T_S1 M <dxid> <amount0> <amount1>
```
- **M**: Mint  
- **dxid**: Unique swap identifier  
- **amount0** and **amount1**: Amounts of tokens `t0` and `t1` to add as liquidity (these tokens are defined during swap deployment).  

### 3. Swap Swap
```
DX T S <dxid> <0/1> <amount>
```
- **S**: Swap  
- **dxid**: Unique swap identifier  
- **0/1**: Indicates whether the swap is for token `t0` or token `t1`.  
- **amount**: The amount being swapped.

### 4. Swap Burn (Remove Liquidity)
```
DX T_S1 B <txid>
```
- **B**: Burn  
- **txid**: The transaction ID after which all liquidity is removed.

---

The complete system code details will be made public after testing is completed.
