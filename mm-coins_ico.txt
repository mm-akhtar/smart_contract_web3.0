// SPDX-License-Identifier: GPL-3.0
// mm-coin ICO

//Version of compiler
pragma solidity ^0.8.4;

contract mmcoin_ico {

    // Introducing the total number of mmcoin available for sale
    uint public max_mmcoins = 1000000;

    // Introducing the USD conversion rate
    uint public usd_to_mmcoins = 1000;

    // Introducing the total number of mmcoin that havr een bought by the investors
    uint public total_mmcoins_bought = 0;

    // Mapping from the investor address to its equity in mmcoin and USD
    mapping(address => uint) equity_mmcoins;
    mapping(address => uint) equity_usd;

    // Checking if an investor can buy mmcoins
    modifier can_buy_mmcoins(uint usd_invested) {
        require(usd_invested * usd_to_mmcoins + total_mmcoins_bought <= max_mmcoins);
        _;
    }

    // Getting the equity in mmcoins of an investor
    function equity_in_mmcoins(address investor) public view returns (uint) {
        return equity_mmcoins[investor];
    }

    // Getting the equity in usd of an invester
    function equity_in_usd(address investor) public view returns (uint) {
        return equity_usd[investor];
    }

    // Buy mmcoins
    function buy_mmcoins(address investor, uint usd_invested) external
    can_buy_mmcoins(usd_invested) {
        uint mmcoins_bought = usd_invested * usd_to_mmcoins;
        equity_mmcoins[investor] += mmcoins_bought;
        equity_usd[investor] = equity_mmcoins[investor] / 1000;
        total_mmcoins_bought += mmcoins_bought;
    }

    // Selling mmcoins
    function sell_mmcoins(address investor, uint mmcoins_sold) external {
        equity_mmcoins[investor] -= mmcoins_sold;
        equity_usd[investor] = equity_mmcoins[investor] / 1000;
        total_mmcoins_bought -= mmcoins_sold;
    }

}