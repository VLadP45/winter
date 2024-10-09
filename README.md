# winter// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract WinterWonderland {

    struct Snowman {
        string name;
        uint256 size;   // Snowman size depends on snowflakes used to build it
    }

    mapping(address => uint256) public snowflakesBalance;
    mapping(address => Snowman[]) public snowmenCollection;

    event SnowflakesCollected(address indexed user, uint256 amount);
    event SnowmanBuilt(address indexed user, string name, uint256 size);

    // Collect snowflakes (earn points)
    function collectSnowflakes(uint256 _amount) public {
        require(_amount > 0, "Must collect at least 1 snowflake");
        snowflakesBalance[msg.sender] += _amount;
        emit SnowflakesCollected(msg.sender, _amount);
    }

    // Build a snowman using collected snowflakes
    function buildSnowman(string memory _name, uint256 _size) public {
        require(snowflakesBalance[msg.sender] >= _size, "Not enough snowflakes to build a snowman");

        // Deduct the snowflakes used to build the snowman
        snowflakesBalance[msg.sender] -= _size;

        // Create and store the snowman
        snowmenCollection[msg.sender].push(Snowman({
            name: _name,
            size: _size
        }));

        emit SnowmanBuilt(msg.sender, _name, _size);
    }

    // View the total number of snowflakes a player has
    function getSnowflakesBalance() public view returns (uint256) {
        return snowflakesBalance[msg.sender];
    }

    // View all snowmen built by a player
    function getSnowmen() public view returns (Snowman[] memory) {
        return snowmenCollection[msg.sender];
    }
}
