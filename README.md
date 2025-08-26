# Ethers.js + Otterscan

[![npm (tag)](https://img.shields.io/npm/v/@cowrieio/ethers-otterscan)](https://www.npmjs.com/package/@cowrieio/ethers-otterscan)
![npm bundle size (version)](https://img.shields.io/bundlephobia/minzip/@cowrieio/ethers-otterscan)
![npm (downloads)](https://img.shields.io/npm/dm/@cowrieio/ethers-otterscan)

-----

**A fork of [Ethers.js](https://github.com/ethers-io/ethers.js) with added support for [Otterscan](https://github.com/otterscan/otterscan) / Erigon's OTS namespace.**

This package extends the popular Ethers.js library with specialized provider methods for interacting with Erigon nodes that expose the `ots_*` JSON-RPC methods. These methods provide efficient access to blockchain explorer data without requiring external indexers.

## 🆕 Otterscan Features

The `OtterscanProvider` class extends `JsonRpcProvider` with the following additional methods:

- **Transaction Analysis**
  - `traceTransaction()` - Get detailed execution traces
  - `getInternalOperations()` - Get internal operations (transfers, creates, etc.)
  - `getTransactionErrorData()` - Get raw revert data for failed transactions
  - `getTransactionRevertReason()` - Get human-readable revert reasons

- **Block Information**  
  - `getBlockDetails()` - Get block data with issuance and fee information
  - `getBlockTransactions()` - Get paginated block transactions with receipts

- **Address History**
  - `searchTransactionsBefore()` - Search transactions before a block
  - `searchTransactionsAfter()` - Search transactions after a block  
  - `iterateAddressHistory()` - Async iterator for transaction history

- **Utilities**
  - `hasCode()` - Efficiently check if address is a contract
  - `getTransactionBySenderAndNonce()` - Find transaction by sender/nonce
  - `getContractCreator()` - Get contract creation details
  - `otsApiLevel()` - Check supported OTS API level

## Installation

```bash
npm install @cowrieio/ethers-otterscan
```

## Usage

```typescript
import { OtterscanProvider } from '@cowrieio/ethers-otterscan';

// Connect to an Erigon node with OTS namespace enabled
const provider = new OtterscanProvider('https://your-erigon-node.com');

// Check OTS availability
await provider.ensureOts(8); // Requires API level 8+

// Get transaction trace
const traces = await provider.traceTransaction('0x...');
console.log(traces); // Array of trace entries

// Search address history between blocks
for await (const { tx, receipt } of provider.iterateAddressHistory(
  '0x...', 
  19000000,  // start block
  19010000   // end block
)) {
  console.log(`Transaction: ${tx.hash}`);
}

// Get internal operations
const ops = await provider.getInternalOperations('0x...');
console.log(ops); // Array of internal operations
```

## TypeScript Support

Full TypeScript support with proper types for all OTS methods:

```typescript
import { OtterscanProvider, type OtsTraceEntry } from '@cowrieio/ethers-otterscan';

const provider = new OtterscanProvider('https://your-erigon-node.com');
const traces: OtsTraceEntry[] = await provider.traceTransaction('0x...');
```

## Compatibility

- **Ethers.js**: Based on Ethers.js v6.15.0
- **Erigon**: Compatible with Erigon nodes running OTS namespace
- **Networks**: Works with any EVM-compatible network running Erigon

## Requirements

Your Erigon node must be configured with the OTS namespace enabled:

```bash
# Start Erigon with OTS namespace
erigon --http.api "eth,erigon,trace,ots" --http --http.addr "0.0.0.0"
```

## Upstream Compatibility

This fork maintains full compatibility with the upstream Ethers.js library. All existing Ethers.js functionality works exactly as documented in the [official Ethers.js documentation](https://docs.ethers.org).

## Differences from Upstream

- ✅ **Added**: `OtterscanProvider` class with OTS namespace support
- ✅ **Added**: TypeScript interfaces for all OTS response types
- ✅ **Added**: Comprehensive test coverage for OTS methods
- ✅ **No breaking changes** to existing Ethers.js functionality

## Testing Nodes

OTS methods are supported on any Erigon node with OTS namespace enabled in launch arguments.

## Documentation

- [Ethers.js Documentation](https://docs.ethers.org/v6/) - All standard Ethers.js functionality
- [Otterscan Documentation](https://docs.otterscan.io/) - Details about OTS methods
- [Erigon Documentation](https://github.com/ledgerwatch/erigon) - Node configuration

## Contributing

This is a community-maintained fork. Contributions are welcome!

1. Fork the repository
2. Make your changes
3. Add tests for new functionality
4. Submit a pull request

## Relationship to Upstream

This package is a fork of [ethers-io/ethers.js](https://github.com/ethers-io/ethers.js) with additional functionality. It is not officially endorsed by the Ethers.js maintainers.

- **Upstream**: `ethers` - The official Ethers.js library
- **This fork**: `@cowrieio/ethers-otterscan` - Community fork with Otterscan support

## License

MIT License (including **all** dependencies), same as upstream Ethers.js.

## Credits

- **[Ethers.js](https://github.com/ethers-io/ethers.js)** by Richard Moore - The amazing library this fork is based on
- **[Otterscan](https://github.com/otterscan/otterscan)** - The blockchain explorer that defines the OTS namespace
- **[Erigon](https://github.com/ledgerwatch/erigon)** - The Ethereum client that implements OTS methods