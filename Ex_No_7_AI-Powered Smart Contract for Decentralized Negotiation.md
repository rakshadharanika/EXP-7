# Experiment 7: AI-Powered Smart Contract for Decentralized Negotiation
### Name  : V Raksha Dharanika
### Reg No: 212223230167
# Aim:
  To create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model.

# Algorithm:
1.The seller lists an item by specifying a base price, a minimum acceptable price, and a maximum negotiation price.

2.A buyer submits an offer by sending the proposed price along with the transaction.

3.The contract uses a simulated AI logic to evaluate the offer using market demand, past transaction data, and time-based price variation.

4.If the offer is acceptable (above the AI-evaluated threshold), the item is sold and payment is transferred to the seller.

5.If the offer is too low but within the negotiation range, the contract generates a counteroffer and refunds the buyer.

6.Each successful transaction influences future pricing by updating internal logic based on accepted prices and negotiation patterns.

# Program:
```py
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AIPoweredNegotiation {
    struct Item {
        address seller;
        uint256 minPrice;
        uint256 maxPrice;
        uint256 basePrice;
        bool sold;
    }

    mapping(uint256 => Item) public items;
    uint256 public itemCount;

    event ItemListed(uint256 itemId, uint256 basePrice);
    event OfferMade(uint256 itemId, address buyer, uint256 offer);
    event CounterOffer(uint256 itemId, uint256 counterOffer);
    event Sold(uint256 itemId, address buyer, uint256 finalPrice);

    function listItem(uint256 _basePrice, uint256 _minPrice, uint256 _maxPrice) public {
        require(_minPrice <= _basePrice && _basePrice <= _maxPrice, "Invalid price range");
        
        items[itemCount] = Item(msg.sender, _minPrice, _maxPrice, _basePrice, false);
        emit ItemListed(itemCount, _basePrice);
        itemCount++;
    }

    function makeOffer(uint256 _itemId, uint256 _offerPrice) public payable {
        Item storage item = items[_itemId];
        require(!item.sold, "Item already sold");
        require(msg.value == _offerPrice, "Incorrect offer amount");

        emit OfferMade(_itemId, msg.sender, _offerPrice);

        uint256 aiCounterOffer = dynamicPricing(item.basePrice, item.minPrice, item.maxPrice, _offerPrice);

        if (_offerPrice >= aiCounterOffer) {
            item.sold = true;
            payable(item.seller).transfer(_offerPrice);
            emit Sold(_itemId, msg.sender, _offerPrice);
        } else {
            payable(msg.sender).transfer(_offerPrice); // Refund buyer
            emit CounterOffer(_itemId, aiCounterOffer);
        }
    }

    function dynamicPricing(uint256 base, uint256 min, uint256 max, uint256 offer) private pure returns (uint256) {
        if (offer >= max) return max;
        if (offer >= base) return base;
        uint256 counter =(base+offer)/2;
        return counter < min ? min : counter; // Enforce a Minimum bound
        
    }
}
```

# Output:
Buyers submit offers, and the contract auto-negotiates the price.



![image](https://github.com/user-attachments/assets/71d6ac3b-fe82-4752-a805-a9e42155976a)




If the buyer’s offer is fair, the deal is executed.




![image](https://github.com/user-attachments/assets/e2786d7e-d47a-4f13-94bf-f85677547d66)





If the buyer’s offer meets or exceeds the AI-evaluated threshold, the transaction is successful.





![image](https://github.com/user-attachments/assets/19bd0978-710e-4718-a93d-c0ede8922bf9)





# High-Level Overview:
1.First-of-its-kind AI-powered pricing contract.


2.Mimics real-world price negotiations using dynamic on-chain pricing.


3.Can be extended to AI oracles for real-time market data.


4.Inspired by AI-enhanced commerce and eBay-like decentralized auctions.

# RESULT:

Thus,To create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model.

