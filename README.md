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
