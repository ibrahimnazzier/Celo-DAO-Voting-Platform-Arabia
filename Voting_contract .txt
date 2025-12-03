// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

/**
 * @title Voting
 * @dev Decentralized Voting Smart Contract for DAO Governance
 * @notice This contract manages proposals and voting for a DAO on Celo blockchain
 */
contract Voting {
    
    // ==================== STRUCTS ====================
    
    struct Proposal {
        uint256 id;
        string title;
        string description;
        uint256 yesVotes;
        uint256 noVotes;
        bool isActive;
        uint256 createdAt;
        address creator;
    }
    
    // ==================== STATE VARIABLES ====================
    
    // Mapping from proposal ID to Proposal struct
    mapping(uint256 => Proposal) public proposals;
    
    // Mapping to track if an address has voted on a proposal: proposalId => voter => hasVoted
    mapping(uint256 => mapping(address => bool)) public hasVoted;
    
    // Total number of proposals created
    uint256 public proposalCount;
    
    // Contract owner (DAO admin)
    address public owner;
    
    // ==================== EVENTS ====================
    
    event ProposalCreated(
        uint256 indexed proposalId,
        string title,
        address indexed creator,
        uint256 timestamp
    );
    
    event Voted(
        address indexed voter,
        uint256 indexed proposalId,
        bool support,
        uint256 timestamp
    );
    
    event ProposalClosed(
        uint256 indexed proposalId,
        uint256 yesVotes,
        uint256 noVotes,
        uint256 timestamp
    );
    
    // ==================== MODIFIERS ====================
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }
    
    modifier proposalExists(uint256 _proposalId) {
        require(_proposalId < proposalCount, "Proposal does not exist");
        _;
    }
    
    modifier proposalActive(uint256 _proposalId) {
        require(proposals[_proposalId].isActive, "Proposal is not active");
        _;
    }
    
    modifier hasNotVoted(uint256 _proposalId) {
        require(!hasVoted[_proposalId][msg.sender], "You have already voted on this proposal");
        _;
    }
    
    // ==================== CONSTRUCTOR ====================
    
    constructor() {
        owner = msg.sender;
        
        // Create initial proposals for testing
        _createProposal(
            "Fund Project A: DeFi Integration",
            "Allocate $50,000 from the treasury to develop a comprehensive DeFi integration module for seamless cross-chain operations. This will enable users to interact with multiple DeFi protocols directly from our platform."
        );
        
        _createProposal(
            "Change Protocol Fee Structure",
            "Reduce transaction fees from 0.5% to 0.3% to improve competitiveness and attract more users to the platform. This change is expected to increase transaction volume by 40% based on market analysis."
        );
        
        _createProposal(
            "Marketing Campaign Budget",
            "Approve $30,000 budget for Q1 2026 marketing campaign focused on social media and community growth initiatives. The campaign will target emerging markets in Southeast Asia and Latin America."
        );
        
        _createProposal(
            "Add New Governance Member",
            "Elect Alice Walker (0x742d...3f4a) as a new core governance member with voting rights on protocol upgrades. Alice has 10+ years of blockchain experience and has contributed significantly to the community."
        );
    }
    
    // ==================== PUBLIC FUNCTIONS ====================
    
    /**
     * @dev Create a new proposal
     * @param _title The title of the proposal
     * @param _description Detailed description of the proposal
     */
    function createProposal(
        string memory _title,
        string memory _description
    ) public onlyOwner {
        _createProposal(_title, _description);
    }
    
    /**
     * @dev Internal function to create a proposal
     */
    function _createProposal(
        string memory _title,
        string memory _description
    ) internal {
        require(bytes(_title).length > 0, "Title cannot be empty");
        require(bytes(_description).length > 0, "Description cannot be empty");
        
        proposals[proposalCount] = Proposal({
            id: proposalCount,
            title: _title,
            description: _description,
            yesVotes: 0,
            noVotes: 0,
            isActive: true,
            createdAt: block.timestamp,
            creator: msg.sender
        });
        
        emit ProposalCreated(proposalCount, _title, msg.sender, block.timestamp);
        
        proposalCount++;
    }
    
    /**
     * @dev Vote on a proposal
     * @param _proposalId The ID of the proposal to vote on
     * @param _support True for Yes vote, False for No vote
     */
    function vote(uint256 _proposalId, bool _support) 
        public 
        proposalExists(_proposalId)
        proposalActive(_proposalId)
        hasNotVoted(_proposalId)
    {
        // Mark that the address has voted
        hasVoted[_proposalId][msg.sender] = true;
        
        // Update vote counts
        if (_support) {
            proposals[_proposalId].yesVotes++;
        } else {
            proposals[_proposalId].noVotes++;
        }
        
        emit Voted(msg.sender, _proposalId, _support, block.timestamp);
    }
    
    /**
     * @dev Close a proposal (only owner)
     * @param _proposalId The ID of the proposal to close
     */
    function closeProposal(uint256 _proposalId) 
        public 
        onlyOwner 
        proposalExists(_proposalId)
        proposalActive(_proposalId)
    {
        proposals[_proposalId].isActive = false;
        
        emit ProposalClosed(
            _proposalId,
            proposals[_proposalId].yesVotes,
            proposals[_proposalId].noVotes,
            block.timestamp
        );
    }
    
    // ==================== VIEW FUNCTIONS ====================
    
    /**
     * @dev Get detailed information about a proposal
     * @param _proposalId The ID of the proposal
     * @return title The proposal title
     * @return description The proposal description
     * @return yesVotes Number of yes votes
     * @return noVotes Number of no votes
     * @return isActive Whether the proposal is active
     */
    function getProposalInfo(uint256 _proposalId) 
        public 
        view 
        proposalExists(_proposalId)
        returns (
            string memory title,
            string memory description,
            uint256 yesVotes,
            uint256 noVotes,
            bool isActive
        ) 
    {
        Proposal memory proposal = proposals[_proposalId];
        return (
            proposal.title,
            proposal.description,
            proposal.yesVotes,
            proposal.noVotes,
            proposal.isActive
        );
    }
    
    /**
     * @dev Get all proposal IDs
     * @return Array of all proposal IDs
     */
    function getAllProposalIds() public view returns (uint256[] memory) {
        uint256[] memory ids = new uint256[](proposalCount);
        for (uint256 i = 0; i < proposalCount; i++) {
            ids[i] = i;
        }
        return ids;
    }
    
    /**
     * @dev Get all active proposal IDs
     * @return Array of active proposal IDs
     */
    function getActiveProposalIds() public view returns (uint256[] memory) {
        // First, count active proposals
        uint256 activeCount = 0;
        for (uint256 i = 0; i < proposalCount; i++) {
            if (proposals[i].isActive) {
                activeCount++;
            }
        }
        
        // Create array and populate with active proposal IDs
        uint256[] memory activeIds = new uint256[](activeCount);
        uint256 index = 0;
        for (uint256 i = 0; i < proposalCount; i++) {
            if (proposals[i].isActive) {
                activeIds[index] = i;
                index++;
            }
        }
        
        return activeIds;
    }
    
    /**
     * @dev Check if an address has voted on a specific proposal
     * @param _proposalId The proposal ID
     * @param _voter The voter address
     * @return Whether the address has voted
     */
    function hasVotedOnProposal(uint256 _proposalId, address _voter) 
        public 
        view 
        proposalExists(_proposalId)
        returns (bool) 
    {
        return hasVoted[_proposalId][_voter];
    }
    
    /**
     * @dev Get the current vote percentage for a proposal
     * @param _proposalId The proposal ID
     * @return yesPercentage Percentage of yes votes (scaled by 100)
     * @return noPercentage Percentage of no votes (scaled by 100)
     */
    function getVotePercentages(uint256 _proposalId) 
        public 
        view 
        proposalExists(_proposalId)
        returns (uint256 yesPercentage, uint256 noPercentage) 
    {
        Proposal memory proposal = proposals[_proposalId];
        uint256 totalVotes = proposal.yesVotes + proposal.noVotes;
        
        if (totalVotes == 0) {
            return (0, 0);
        }
        
        yesPercentage = (proposal.yesVotes * 10000) / totalVotes; // Scaled by 100 for 2 decimal precision
        noPercentage = (proposal.noVotes * 10000) / totalVotes;
        
        return (yesPercentage, noPercentage);
    }
    
    /**
     * @dev Get proposal result
     * @param _proposalId The proposal ID
     * @return approved Whether the proposal is approved (more yes than no votes)
     */
    function getProposalResult(uint256 _proposalId) 
        public 
        view 
        proposalExists(_proposalId)
        returns (bool approved) 
    {
        Proposal memory proposal = proposals[_proposalId];
        return proposal.yesVotes > proposal.noVotes;
    }
    
    /**
     * @dev Transfer ownership of the contract
     * @param _newOwner Address of the new owner
     */
    function transferOwnership(address _newOwner) public onlyOwner {
        require(_newOwner != address(0), "New owner cannot be zero address");
        owner = _newOwner;
    }
}
