# 🗳️ Celo DAO Voting Platform

A beautiful, production-ready Decentralized Voting (DAO) Application built on the Celo Blockchain. This DApp features a sleek dark mode UI with glassmorphism effects, allowing users to participate in governance by voting on proposals.

## ✨ Features

- **🎨 Professional Dark Mode UI**: Cyberpunk-inspired design with purple/black theme and glassmorphism effects
- **🔗 Celo Wallet Integration**: Seamless connection with MetaMask and automatic network switching to Celo Alfajores Testnet
- **📊 Real-time Voting Dashboard**: View active proposals with live vote counts
- **✅ Secure Voting System**: On-chain voting with double-vote prevention
- **📈 Live Charts**: Visual representation of voting results using Chart.js
- **🚀 GitHub Pages Ready**: Single-file deployment structure
- **📱 Responsive Design**: Works perfectly on desktop and mobile devices

## 🛠️ Tech Stack

- **Frontend**: HTML5, Tailwind CSS, JavaScript
- **Web3**: Ethers.js v5.7.2
- **Blockchain**: Celo Alfajores Testnet
- **Smart Contract**: Solidity ^0.8.19
- **Charts**: Chart.js v4.4.0

## 📁 Project Structure

```
p2/
├── index.html          # Main application (single-page DApp)
├── Voting.sol          # Smart contract source code
└── README.md           # This file
```

## 🚀 Quick Start

### 1. Deploy the Smart Contract

1. Install [Remix IDE](https://remix.ethereum.org/) or use your preferred Solidity development environment
2. Open `Voting.sol` in Remix
3. Compile with Solidity version ^0.8.19
4. Connect MetaMask to **Celo Alfajores Testnet**:
   - Network Name: Celo Alfajores Testnet
   - RPC URL: `https://alfajores-forno.celo-testnet.org`
   - Chain ID: `44787`
   - Currency Symbol: `CELO`
   - Block Explorer: `https://alfajores.celoscan.io/`
5. Get testnet CELO from the [Celo Faucet](https://faucet.celo.org/)
6. Deploy the contract and copy the deployed contract address

### 2. Configure the Frontend

1. Open `index.html` in a text editor
2. Find this line near the top of the `<script>` section:
   ```javascript
   const contractAddress = "PUT_ADDRESS_HERE";
   ```
3. Replace `"PUT_ADDRESS_HERE"` with your deployed contract address:
   ```javascript
   const contractAddress = "0xYourContractAddressHere";
   ```
4. Save the file

### 3. Run the Application

#### Option A: Local Testing
Simply open `index.html` in your web browser (double-click the file)

#### Option B: GitHub Pages Deployment
1. Create a new GitHub repository
2. Upload `index.html` to the repository
3. Go to Settings → Pages
4. Select the branch (usually `main`) and root directory
5. Click Save
6. Your DApp will be live at `https://yourusername.github.io/repository-name/`

## 🎮 How to Use

### Connect Wallet
1. Click the **"Connect Wallet"** button in the sidebar
2. Approve the MetaMask connection
3. The app will automatically switch to Celo Alfajores Testnet (or prompt you to add it)

### View Proposals
- The dashboard displays all active proposals with current vote counts
- Each proposal card shows:
  - Proposal title and description preview
  - Current Yes/No vote counts
  - Active status badge

### Vote on a Proposal
1. Click on any proposal card to view details
2. Review the full proposal description
3. Check the live voting chart showing current results
4. Click **"Vote YES"** or **"Vote NO"**
5. Confirm the transaction in MetaMask
6. Wait for blockchain confirmation
7. Your vote is recorded (you can only vote once per proposal)

## 📝 Smart Contract Details

### Contract Functions

#### Public Functions
- `createProposal(title, description)` - Create a new proposal (owner only)
- `vote(proposalId, support)` - Vote on a proposal (true = Yes, false = No)
- `closeProposal(proposalId)` - Close a proposal (owner only)

#### View Functions
- `getProposalInfo(proposalId)` - Get proposal details
- `hasVotedOnProposal(proposalId, voter)` - Check if address has voted
- `getAllProposalIds()` - Get all proposal IDs
- `getActiveProposalIds()` - Get only active proposal IDs
- `getVotePercentages(proposalId)` - Get vote percentages
- `getProposalResult(proposalId)` - Check if proposal is approved

### Events
- `ProposalCreated` - Emitted when a new proposal is created
- `Voted` - Emitted when a vote is cast
- `ProposalClosed` - Emitted when a proposal is closed

## 🔒 Security Features

- **Double-Vote Prevention**: Smart contract ensures each address can only vote once per proposal
- **Owner-Only Functions**: Proposal creation and closing restricted to contract owner
- **Input Validation**: All inputs are validated on-chain
- **Safe Math**: Built-in overflow protection with Solidity ^0.8.0

## 🎨 UI Features

- **Glassmorphism Design**: Modern frosted glass effect on cards
- **Neon Glow Effects**: Cyberpunk-style glowing elements
- **Smooth Animations**: CSS transitions for all interactions
- **Progress Bars**: Visual representation of vote distribution
- **Live Charts**: Interactive doughnut charts for vote visualization
- **Responsive Layout**: Adapts to all screen sizes

## 🧪 Testing Checklist

- [ ] MetaMask installed and configured
- [ ] Connected to Celo Alfajores Testnet
- [ ] Have testnet CELO tokens
- [ ] Contract deployed successfully
- [ ] Contract address updated in `index.html`
- [ ] Can connect wallet
- [ ] Can view proposals
- [ ] Can vote on proposals
- [ ] Cannot vote twice on same proposal
- [ ] Vote counts update correctly
- [ ] Charts display properly

## 🔗 Useful Links

- [Celo Documentation](https://docs.celo.org/)
- [Celo Alfajores Faucet](https://faucet.celo.org/)
- [Celo Explorer](https://alfajores.celoscan.io/)
- [Ethers.js Documentation](https://docs.ethers.org/v5/)
- [Remix IDE](https://remix.ethereum.org/)

## 📱 Supported Wallets

- MetaMask
- Celo Wallet
- Any Web3-compatible wallet that supports Celo

## 🐛 Troubleshooting

### Wallet won't connect
- Ensure MetaMask is installed
- Check if you're on the correct network
- Try refreshing the page

### Can't switch to Celo Alfajores
- Manually add the network in MetaMask using the RPC details above
- Ensure MetaMask is up to date

### Transaction fails
- Check if you have enough CELO for gas fees
- Verify the contract address is correct
- Ensure you haven't already voted on the proposal

### Contract not responding
- Verify the contract address in `index.html` is correct
- Check if the contract is deployed on Alfajores testnet
- Open browser console (F12) to check for errors

## 🤝 Contributing

This is an MVP/demo project. Feel free to fork and enhance with:
- Additional proposal types
- Voting weight based on token holdings
- Time-locked voting periods
- Delegation features
- Proposal execution logic

## 📄 License

MIT License - Feel free to use this project for learning or production

## 👨‍💻 Developer Notes

### Contract Deployment Steps (Detailed)

1. **Using Remix IDE**:
   ```
   1. Go to remix.ethereum.org
   2. Create new file: Voting.sol
   3. Paste the contract code
   4. Go to "Solidity Compiler" tab
   5. Select compiler version 0.8.19 or higher
   6. Click "Compile Voting.sol"
   7. Go to "Deploy & Run Transactions" tab
   8. Select "Injected Provider - MetaMask"
   9. Ensure MetaMask is on Celo Alfajores
   10. Click "Deploy"
   11. Confirm transaction in MetaMask
   12. Copy deployed contract address
   ```

2. **Using Hardhat** (Advanced):
   ```bash
   npm install --save-dev hardhat @nomiclabs/hardhat-ethers ethers
   npx hardhat init
   # Add Celo Alfajores to hardhat.config.js
   npx hardhat run scripts/deploy.js --network celoAlfajores
   ```

### Frontend Customization

- **Colors**: Edit Tailwind classes in HTML or add custom CSS
- **Proposals**: Modify the `mockProposals` array in JavaScript
- **Network**: Change `CELO_ALFAJORES` constant to use different network
- **Charts**: Customize Chart.js options in `initVotingChart()` function

---

**Built with ❤️ for the Celo Community**

For questions or support, check the Celo Discord or documentation.