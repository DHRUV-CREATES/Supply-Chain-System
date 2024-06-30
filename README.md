# Supply-Chain-System

This contract implements a simple supply chain system. Sellers can create products, buyers can purchase them, and confirm delivery."

Functions Breakdown

Function: createProduct
Sellers can create a new product.
The require statement ensures the price is above zero.

Function: buyProduct
Buyers can purchase a product.
The require statements ensure the product is for sale and the payment is correct.

Function: confirmDelivery
Buyers confirm the delivery of a product.
The require statements ensure the product is paid for and the caller is the buyer.

Function: checkInvariant

# Step-by-Step Guide to Deploy and Run the Contract in Remix
Open Remix IDE:

Go to Remix IDE.
Create a New File:

In the left sidebar, click on the "+" icon to create a new file.
Name your file SupplyChain.sol.
Copy and Paste the Code:

Copy the Solidity code provided above and paste it into the newly created SupplyChain.sol file.
Compile the Contract:

Click on the "Solidity Compiler" tab in the left sidebar (it looks like a "compiler" icon).
Ensure the compiler version is set to 0.8.x (the version specified in the pragma statement).
Click on the "Compile SupplyChain.sol" button.
Deploy the Contract:

Click on the "Deploy & Run Transactions" tab in the left sidebar (it looks like an "Ethereum" icon).
Select "JavaScript VM" as the environment for testing purposes.
Make sure the SupplyChain contract is selected in the "Contract" dropdown.
Click on the "Deploy" button.
Interact with the Contract:

Once deployed, the contract will appear under "Deployed Contracts" in the same tab.
You can interact with the contract using the provided buttons and fields.

# CODE
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SupplyChain {
    enum State { Created, Paid, Delivered }
    
    struct Product {
        string name;
        uint256 price;
        address payable seller;
        address buyer;
        State state;
    }

    mapping(uint256 => Product) public products;
    uint256 public productCount;

    event ProductCreated(uint256 productId, string name, uint256 price, address seller);
    event ProductPaid(uint256 productId, address buyer);
    event ProductDelivered(uint256 productId);

    // Function to create a new product
    function createProduct(string memory _name, uint256 _price) public {
        // Ensure the product price is greater than zero
        require(_price > 0, "Price must be greater than zero");
        
        productCount++;
        products[productCount] = Product({
            name: _name,
            price: _price,
            seller: payable(msg.sender),
            buyer: address(0),
            state: State.Created
        });

        emit ProductCreated(productCount, _name, _price, msg.sender);
    }

    // Function to buy a product
    function buyProduct(uint256 _productId) public payable {
        Product storage product = products[_productId];
        
        // Ensure the product is available for sale and the payment is correct
        require(product.state == State.Created, "Product is not available for sale");
        require(msg.value == product.price, "Incorrect payment amount");

        product.buyer = msg.sender;
        product.state = State.Paid;

        emit ProductPaid(_productId, msg.sender);
    }

    // Function to confirm delivery of the product
    function confirmDelivery(uint256 _productId) public {
        Product storage product = products[_productId];
        
        // Ensure the product has been paid for and the caller is the buyer
        require(product.state == State.Paid, "Product is not yet paid for");
        require(product.buyer == msg.sender, "Only the buyer can confirm delivery");

        product.state = State.Delivered;
        product.seller.transfer(product.price);

        emit ProductDelivered(_productId);
    }

    // Internal function to check contract balance consistency
    function checkInvariant() public view {
        uint256 totalValue = 0;
        for (uint256 i = 1; i <= productCount; i++) {
            if (products[i].state == State.Paid) {
                totalValue += products[i].price;
            }
        }
        // Ensure the contract's balance covers all unpaid products
        assert(address(this).balance >= totalValue);
    }
}

