// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract HalfToken {
    string public name = "HALF Token";
    string public symbol = "HALF";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    
    uint256 public constant INITIAL_PRICE_PER_TOKEN = 0.0001 ether; // Assuming 1 ether = 1 BTC for simplicity
    uint256 public constant SET_SIZE = 1e8; // 100,000 tokens in a set
    uint256 public constant INITIAL_SETS = 10000000000; // 10 billion sets
    uint256 public constant HALVING_PERIOD = 5730 minutes;  // 3.7 days
    uint256 public lastHalvingTime;
    
    mapping(address => uint256) public balances;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Mint(address indexed to, uint256 amount);
    
    constructor() {
        totalSupply = INITIAL_SETS * SET_SIZE;
        lastHalvingTime = block.timestamp;
        balances[msg.sender] = totalSupply; // Allocate all tokens to the contract creator initially
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balances[msg.sender] >= _value, "Insufficient balance.");
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function remainingSets() public view returns (uint256) {
        uint256 elapsedTime // SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract HalfToken {
    string public name = "HALF Token";
    string public symbol = "HALF";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    
    uint256 public constant INITIAL_PRICE_PER_TOKEN = 0.0001 ether; // Assuming 1 ether = 1 BTC for simplicity
    uint256 public constant SET_SIZE = 1e8; // 100,000 tokens in a set
    uint256 public constant INITIAL_SETS = 10000000000; // 10 billion sets
    uint256 public constant HALVING_PERIOD = 5730 minutes;  // 3.7 days
    uint256 public lastHalvingTime;
    
    mapping(address => uint256) public balances;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Mint(address indexed to, uint256 amount);
    
    constructor() {
        totalSupply = INITIAL_SETS * SET_SIZE;
        lastHalvingTime = block.timestamp;
        balances[msg.sender] = totalSupply; // Allocate all tokens to the contract creator initially
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balances[msg.sender] >= _value, "Insufficient balance.");
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function remainingSets() public view returns (uint256) {
        uint256 elapsedTime = block.timestamp - lastHalvingTime;
        uint256 halvings = elapsedTime / HALVING_PERIOD;
        return INITIAL_SETS / (2 ** halvings);
    }

    function adjustedPrice() public view returns (uint256) {
        return INITIAL_PRICE_PER_TOKEN * (INITIAL_SETS * SET_SIZE) / (remainingSets() * SET_SIZE);
    }

    function mintSet() public {
        require(block.timestamp >= lastHalvingTime + HALVING_PERIOD, "Halving period not yet complete.");
        
        uint256 newSets = remainingSets();
        require(newSets > 0, "No sets available for minting.");
        
        // Mint new tokens
        uint256 amountToMint = newSets * SET_SIZE;
        totalSupply += amountToMint;
        balances[msg.sender] += amountToMint;
        
        lastHalvingTime = block.timestamp; // Reset halving timer
        emit Mint(msg.sender, amountToMint);
    }

    function balanceOf(address _owner) public view returns (uint256 balance) {
        return balances[_owner];
    }
}
= block.timestamp - lastHalvingTime;
        uint256 halvings = elapsedTime / HALVING_PERIOD;
        return INITIAL_SETS / (2 ** halvings);
    }

    function adjustedPrice() public view returns (uint256) {
        return INITIAL_PRICE_PER_TOKEN * (INITIAL_SETS * SET_SIZE) / (remainingSets() * SET_SIZE);
    }

    function mintSet() public {
        require(block.timestamp >= lastHalvingTime + HALVING_PERIOD, "Halving period not yet complete.");
        
        uint256 newSets = remainingSets();
        require(newSets > 0, "No sets available for minting.");
        
        // Mint new tokens
        uint256 amountToMint = newSets * SET_SIZE;
        totalSupply += amountToMint;
        balances[msg.sender] += amountToMint;
        
        lastHalvingTime = block.timestamp; // Reset halving timer
        emit Mint(msg.sender, amountToMint);
    }

    function balanceOf(address _owner) public view returns (uint256 balance) {
        return balances[_owner];
    }
}


