
# Multi-Chain Decentralized Exchange (DEX) Aggregator

## Vision
To create a comprehensive and efficient decentralized exchange aggregator that seamlessly integrates multiple blockchains, providing users with optimal trading rates and liquidity using Chainflip SDK.

## Project Overview
The Multi-Chain DEX Aggregator leverages Chainflip SDK to aggregate liquidity from various decentralized exchanges across different blockchains. This project aims to provide users with the best possible trading rates, enhance liquidity, and simplify the cross-chain trading experience.

## Key Features
1. **Multi-Chain Liquidity Aggregation**
   - Aggregates liquidity from various decentralized exchanges across multiple blockchains.
   - Utilizes Chainflip SDK to facilitate cross-chain swaps and ensure optimal trading rates.

2. **Optimized Trading Experience**
   - Provides users with real-time price quotes and trading opportunities.
   - Minimizes slippage and ensures efficient trade execution.

3. **User-Friendly Interface**
   - Intuitive interface for seamless trading and liquidity management.
   - Displays real-time data and analytics for informed trading decisions.

## Technical Stack
- **Frontend**: React.js, Next.js
- **Backend**: Node.js, Express.js
- **Blockchain Integration**: Chainflip SDK
- **Database**: MongoDB
- **Deployment**: Docker, Kubernetes, AWS/GCP

## Chainflip SDK Integration
The project uses the Chainflip SDK to handle cross-chain liquidity aggregation and trading. Specifically, the integration is demonstrated in the `Exchange.tsx` file.

### `Exchange.tsx`
The following code snippet shows how the Chainflip SDK is used in the project:

```typescript
import React, { useEffect, useState } from 'react';
import { SwapSDK, ChainflipNetwork } from "@chainflip/sdk/swap";
import { Wallet } from "ethers";

type ChainflipNetwork = 'mainnet' | 'testnet' | 'perseverance';

interface SwapComponentProps {
  sellValue: number;
  buyValue: number;
  priceChange: number;
  exchangeRate: number; // The exchange rate from sellCurrency to buyCurrency
}

const ExchangeComponent: React.FC<SwapComponentProps> = ({
  sellValue,
  buyValue,
  priceChange,
  exchangeRate,
}) => {
  const [sellAmount, setSellAmount] = useState<number>(0); // Pre-filled sell amount
  const [sellCurrency, setSellCurrency] = useState<string>('FLIP');
  const [buyCurrency, setBuyCurrency] = useState<string>('ARB');
  const [chains, setChains] = useState<any[]>([]);

  const options = {
    network: "perseverance" as ChainflipNetwork, // Testnet
    backendServiceUrl: "https://example.chainflip.io",
    signer: Wallet.fromPhrase("fold unique social wild fury mammal strike rule pet observe mule curve"),
    broker: {
      url: 'https://my.broker.io',
      commissionBps: 0, // basis points, i.e. 100 = 1%
    },
  };
   
  const swapSDK = new SwapSDK(options);

  const getChains = async () => {
    const res = await swapSDK.getChains();
    return res;
  };

  useEffect(() => {
    const func = async () => {
      const res = await getChains();
      console.log(res);
      setChains(res);
      return res;
    };
    func();
  }, []);

  const handleSellAmountChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setSellAmount(parseFloat(e.target.value));
  };

  const handleSellCurrencyChange = (e: React.ChangeEvent<HTMLSelectElement>) => {
    setSellCurrency(e.target.value);
  };

  const handleBuyCurrencyChange = (e: React.ChangeEvent<HTMLSelectElement>) => {
    setBuyCurrency(e.target.value);
  };

  const buyAmount = sellAmount * exchangeRate;

  return (
    <div className="max-w-md mx-auto bg-gray-900 text-white p-6 rounded-lg shadow-lg dark:bg-gra dark:text-gray-200">
      <div className="flex justify-between items-center mb-2">
        <div className='p-2'>
          <h2 className="text-xl font-bold">Sell</h2>
          <input
            type="number"
            value={sellAmount}
            onChange={handleSellAmountChange}
            className="text-3xl bg-gray-800 p-2 rounded  w-full dark:bg-gray-700"
          />
          <p className="text-lg">${(sellAmount * sellValue).toFixed(2)!=='NaN'?(sellAmount * sellValue).toFixed(2):'0.00'}</p>
        </div>
        <div className='p-2'>
          <select
            value={sellCurrency}
            onChange={handleSellCurrencyChange}
            className="bg-blue-500 px-3 py-1 rounded-full dark:bg-blue-700"
          >
            <option value="ETH">FLIP</option>
            <option value="BTC">ETH</option>
            <option value="USDT">USDC</option>
            {/* Add more options as needed */}
          </select>
        </div>
      </div>
      <div className="flex justify-center my-4">
        <div className="bg-gray-800 p-2 rounded-full dark:bg-gray-700">
          <span className="text-2xl">↓</span>
        </div>
      </div>
      <div className="flex justify-between items-center mb-4">
        <div className="p-2">
          <h2 className="text-xl font-bold">Buy</h2>
          <p className="text-3xl">{buyAmount.toFixed(2)!=='NaN'?buyAmount.toFixed(2):'0.00'}</p>
          <p className="text-lg">
            ${(buyAmount * buyValue).toFixed(2)!=='NaN'?(buyAmount * buyValue).toFixed(2):'0.00'}{' '}
            <span className={`text-lg ${priceChange < 0 ? 'text-red-500' : 'text-green-500'}`}>
              ({priceChange.toFixed(2)}%)
            </span>
          </p>
        </div>
        <div className="p-2">
          <select
            value={buyCurrency}
            onChange={handleBuyCurrencyChange}
            className="bg-green-500 px-3 py-1 rounded-full dark:bg-green-700"
          >
            <option value="COMP">ARB</option>
            <option value="LINK">USDC</option>
            <option value="DAI">SOL</option>
            {/* Add more options as needed */}
          </select>
        </div>
      </div>
    </div>
  );
};

export default ExchangeComponent;
```

## Usage
Follow these steps to set up the development environment and run the DEX Aggregator.

## Installation
1. Clone the repository:
   ```sh
   git clone https://github.com/your-username/multi-chain-dex-aggregator.git
   cd multi-chain-dex-aggregator
   ```

2. Install dependencies:
   ```sh
   npm install
   ```

3. Start the development server:
   ```sh
   npm start
   ```

## Contributing
### Pre-requisites
```sh
curl -fsSL https://bun.sh/install | bash
```
Copy `.env.example` to `.env` and fill it with your configuration data.

### Installation
```sh
bun bootstrap
```

### Branches
- **main** - Production branch.
- **develop** - Development branch - all PRs should be merged here first.
- **nightly** - Branch for nightly builds - can be used for testing purposes.

### Testing
To run tests, use the following command:
```sh
bun test
```

### Pull Requests
- PRs should be created from the `develop` branch.
- PRs should be reviewed by at least one Code Owner (see `CODEOWNERS` file).
- PRs should include a scope in the commit message (see commit messages section).
- PRs should include tests if possible.
- PRs should include a changeset file if needed (see release section).

### New Package
To create a new package, use the following command and pick one of the options:
```sh
bun generate
```
This command sets up the package with necessary files for bundling and publishing.

### Release and Publish
Packages are automatically published to npm when a new PR is merged to the `main` & `develop` branches. To automate and handle the process, we use changesets and GitHub action workflows.

Before running `bun changeset`, pull the `main` & `develop` branches.

To release a new version of a package, create a PR with the changes and add a changeset file to your commit:
```sh
bun changeset
```

After the PR is merged to the `develop` branch with the changeset file, a GitHub action will create a new PR with updated versions of packages and changelogs.

## Conclusion
The Multi-Chain DEX Aggregator leverages Chainflip SDK to provide a powerful and efficient cross-chain trading experience. By integrating liquidity from multiple decentralized exchanges, it ensures optimal trading rates and enhanced liquidity for users.