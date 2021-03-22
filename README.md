# Solidity `freeTx` modifier

Simple implementation of `freeTx` solidity modifier to add a full transaction cost rebate to any function

## Snippet
```solidity
/** 
 * @title NiceGuy
 * @dev Implements `freeTx` modifier to add a full transaction cost rebate to any function
 */
contract NiceGuy {
    
    /**
     * @dev Transfer all ether use in the contract function back to the sender  
     */
    modifier freeTx() {
        uint256 startGas = gasleft();
        _;
        uint256 spentGas = startGas - gasleft() + 21000 + 16 * msg.data.length;
        payable(msg.sender).transfer(spentGas * tx.gasprice);
    }
    
}
```

## Usage
```solidity
/**
 * @title Button created by Marto
 * @dev Usage example of NiceGuy "freeTx" modifier to add a full transaction cost rebate to any function
 */
contract Button is NiceGuy {
    uint256 public counter = 0;
    
    constructor() payable {}
    
    /**
     * @dev Transfer all ether use in the contract function back to the sender  
     */
    function push() public freeTx {
        // Make some expensive computation
        for (uint i=0; i < 500; i++) {
            counter = counter + 1;
        }
    }
    
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}
```






##
###### Made with ❤ by <a target="_blank" href="https://twitter.com/martinlsanchez" class="author">Marto</a> (marto.eth)
