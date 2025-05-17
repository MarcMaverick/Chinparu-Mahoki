// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract ChinparuToken is ERC20, Ownable {
    uint256 public constant INITIAL_SUPPLY = 500_000_000 * 10 ** 18; // 500 Mio CHIT
    uint256 public constant MAX_SUPPLY = 999_999_999 * 10 ** 18;     // Max. 999.999.999 CHIT

    address public constant userAddress = 0x28F67a16Ebb46Eb01F89e304f4D3D6ddDAe2B1Fa;

    address public projectReserve = userAddress;
    address public partnershipReserve = userAddress;
    address public teamReserve = userAddress;
    address public communityReserve = userAddress;

    constructor() ERC20("Chinparu Token", "CHIT") {
        _mint(msg.sender, INITIAL_SUPPLY * 30 / 100);         // 30% an msg.sender für ICO
        _mint(partnershipReserve, INITIAL_SUPPLY * 25 / 100); // 25% für Partnerschaften
        _mint(projectReserve, INITIAL_SUPPLY * 20 / 100);     // 20% für Umweltprojekte
        _mint(teamReserve, INITIAL_SUPPLY * 15 / 100);        // 15% für Team
        _mint(communityReserve, INITIAL_SUPPLY * 10 / 100);   // 10% für Community
    }

    function mint(address to, uint256 amount) external onlyOwner {
        require(totalSupply() + amount <= MAX_SUPPLY, "Max supply reached");
        _mint(to, amount);
    }

    function updateReserves(
        address _projectReserve,
        address _partnershipReserve,
        address _teamReserve,
        address _communityReserve
    ) external onlyOwner {
        require(_projectReserve != address(0), "Invalid address");
        require(_partnershipReserve != address(0), "Invalid address");
        require(_teamReserve != address(0), "Invalid address");
        require(_communityReserve != address(0), "Invalid address");

        projectReserve = _projectReserve;
        partnershipReserve = _partnershipReserve;
        teamReserve = _teamReserve;
        communityReserve = _communityReserve;
    }

    function transferOwnership(address newOwner) public override onlyOwner {
        require(newOwner != address(0), "New owner: zero address");
        super.transferOwnership(newOwner);
    }
}
