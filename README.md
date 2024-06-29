# Supply-Chain-System

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SupplyChain {
    address public owner;
    string public ownerName;
    uint256 public productCount = 0;
   
  enum State { Manufactured, Shipped, Delivered }
    struct Product {
        uint256 id;
        string name;
        uint256 price;
        State state;
        address payable manufacturer;
        string manufacturerName;
        address payable currentOwner;
        string productDetails;
        uint256 cost;
    }

  mapping(uint256 => Product) public products;

  event ProductManufactured(uint256 id, string name, uint256 price, address indexed manufacturer, string manufacturerName);
    event ProductShipped(uint256 id, address indexed shipper);
    event ProductDelivered(uint256 id, address indexed customer);

  constructor() {
        owner = msg.sender;
        ownerName = "Dhruv";

   // Adding sample products with Rajat as the manufacturer
        manufactureProduct("Product A", 100, "Rajat", "Details of Product A", 80);
        manufactureProduct("Product B", 200, "Rajat", "Details of Product B", 150);
        manufactureProduct("Product C", 300, "Rajat", "Details of Product C", 250);
    }

  modifier onlyManufacturer(uint256 _productId) {
        require(products[_productId].manufacturer == msg.sender, "Only the manufacturer can perform this action");
        _;
    }

  modifier inState(uint256 _productId, State _state) {
        require(products[_productId].state == _state, "Invalid state");
        _;
    }

  function manufactureProduct(string memory _name, uint256 _price, string memory _manufacturerName, string memory _productDetails, uint256 _cost) public {
        require(_price > 0, "Price must be greater than zero");
        require(_cost > 0, "Cost must be greater than zero");

   productCount++;
        products[productCount] = Product({
            id: productCount,
            name: _name,
            price: _price,
            state: State.Manufactured,
            manufacturer: payable(msg.sender),
            manufacturerName: _manufacturerName,
            currentOwner: payable(msg.sender),
            productDetails: _productDetails,
            cost: _cost
        });

  emit ProductManufactured(productCount, _name, _price, msg.sender, _manufacturerName);
    }

 function shipProduct(uint256 _productId) public onlyManufacturer(_productId) inState(_productId, State.Manufactured) {
        products[_productId].state = State.Shipped;

   emit ProductShipped(_productId, msg.sender);
    }

 function deliverProduct(uint256 _productId, address payable _customer) public onlyManufacturer(_productId) inState(_productId, State.Shipped) {
        products[_productId].state = State.Delivered;
        products[_productId].currentOwner = _customer;

   emit ProductDelivered(_productId, _customer);
    }

  function getProduct(uint256 _productId) public view returns (uint256, string memory, uint256, State, address, string memory, address, string memory, uint256) {
        Product memory product = products[_productId];
        return (
            product.id,
            product.name,
            product.price,
            product.state,
            product.manufacturer,
            product.manufacturerName,
            product.currentOwner,
            product.productDetails,
            product.cost
        );
    }
}
