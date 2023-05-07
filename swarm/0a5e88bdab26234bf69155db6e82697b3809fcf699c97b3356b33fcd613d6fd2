// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/draft-ERC20Permit.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Votes.sol";
import "@openzeppelin/contracts/access/AccessControl.sol";

contract YogaResearchInstitute is ERC20, ERC20Burnable, ERC20Permit, ERC20Votes, AccessControl {
    bytes32 public constant TREASURY_ROLE = keccak256("TREASURY_ROLE");

    uint256 public immutable MAX_SUPPLY = 20_000_000_000 * 10**decimals();
    uint256 public immutable TREASURY_AMOUNT = 10_000_000_000 * 10**decimals();

    constructor() ERC20("YogaResearchInstitute", "YOGI") ERC20Permit("YogaResearchInstitute") {
        _setupRole(DEFAULT_ADMIN_ROLE, msg.sender);
        _setupRole(TREASURY_ROLE, msg.sender);
        _mint(msg.sender, MAX_SUPPLY - TREASURY_AMOUNT);
        _mintTreasury(TREASURY_AMOUNT);
    }

    function _mintTreasury(uint256 amount) internal {
        _mint(address(this), amount);
        grantRole(TREASURY_ROLE, address(this));
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        if (hasRole(TREASURY_ROLE, msg.sender)) {
            require(balanceOf(address(this)) - amount >= TREASURY_AMOUNT, "Insufficient treasury balance");
        }
        return super.transfer(recipient, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        if (hasRole(TREASURY_ROLE, sender)) {
            require(balanceOf(address(this)) - amount >= TREASURY_AMOUNT, "Insufficient treasury balance");
        }
        return super.transferFrom(sender, recipient, amount);
    }

    function _afterTokenTransfer(address from, address to, uint256 amount) internal override(ERC20, ERC20Votes) {
        super._afterTokenTransfer(from, to, amount);
    }

    function _mint(address to, uint256 amount) internal override(ERC20, ERC20Votes) {
        require(totalSupply() + amount <= MAX_SUPPLY, "Total supply cannot exceed max supply");
        super._mint(to, amount);
    }

    function _burn(address account, uint256 amount) internal override(ERC20, ERC20Votes) {
        super._burn(account, amount);
    }
}
