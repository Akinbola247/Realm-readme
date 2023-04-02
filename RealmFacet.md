# Aavegotchi
Aavegotchi is a crypto collectible game built on Ethereum where participants can purchase and grow Aavegotchis. Aavegotchi is a decentralized application built on the Ethereum blockchain that combines NFTs (non-fungible tokens) with DeFi (decentralized finance) gameplay mechanics.

# Realm overview
A realm refers to a unique virtual dimension within the Aavegotchi game. Each realm has its own distinct set of rules, properties, and characteristics that determine the types of Aavegotchis that can exist within it and the mechanics of gameplay.

## Imports:
```
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "../../libraries/AppStorage.sol";
import "../../libraries/LibDiamond.sol";
import "../../libraries/LibStrings.sol";
import "../../libraries/LibMeta.sol";
import "../../libraries/LibERC721.sol";
import "../../libraries/LibRealm.sol";
import "../../libraries/LibAlchemica.sol";
import {InstallationDiamondInterface} from "../../interfaces/InstallationDiamondInterface.sol";
import "../../libraries/LibSignature.sol";
import "../../interfaces/IERC1155Marketplace.sol";
```

## Inheritances
```
contract RealmFacet is Modifiers {}
```
Inclusive in the modifier is the App storage which is inherited by the Realm facet and every other facets in the realm diamond.

## Declarations
* The MintParcelInput struct: A pointer to the details of newly minted parcels.
Note : Installations are special structures built on top of your REALM Parcel. Installations are crafted via various combinations of Alchemica, and can be freely traded for GHST in the Aavegotchi Baazaar.
* event EquipInstallation : add new structure type
* event UnequipInstallation :remove a structure type 
* event EquipTile :  adding a tile, The tile is a single square on the grid, represented by a small square icon
* event UnequipTile : emits when a tile is being removed
* event AavegotchiDiamondUpdated: (this event was declared but never used in the facet)
* event InstallationUpgraded : emits when a previous installation has been upgraded to an entirely new one

## Functions
```
function mintParcels(address[] calldata _to, uint256[] calldata _tokenIds, MintParcelInput[] memory _metadata) external onlyOwner;
```
**mintParcels** Allows diamond owner to mint new Parcels.

```
function equipInstallation( uint256 _realmId, uint256 _gotchiId, uint256 _installationId, uint256 _x, uint256 _y, bytes memory _signature) public gameActive canBuild;
```

**equipInstallation** Allow a parcel owner to equip an installation, I.e structures built on realm parcels.

```
function unequipInstallation(uint256 _realmId, uint256 _gotchiId, //will be used soon, uint256 _installationId, uint256 _x, uint256 _y, bytes memory _signature) public onlyParcelOwner(_realmId) gameActive canBuild

```
**unequipInstallation** Allows a parcel owner to unequip an installation, The _x and _y denote the size of the installation and are used to make sure that slot is available on a parcel.

```
function moveInstallation ( uint256 _realmId, uint256 _installationId, uint256 _x0, uint256 _y0, uint256 _x1, uint256 _y1) external onlyParcelOwner(_realmId) gameActive canBuild
```
**moveInstallation** Allows a parcel owner to move an installation to another coordinate.

```
function unequipTile( uint256 _realmId, uint256 _gotchiId, uint256 _tileId, uint256 _x, uint256 _y, bytes memory _signature) public onlyParcelOwner(_realmId) gameActive canBuild
```
**unequipTile** Allows a parcel owner to unequip a tile

```
function moveTile( uint256 _realmId, uint256 _tileId, uint256 _x0, uint256 _y0, uint256 _x1, uint256 _y1
  ) external onlyParcelOwner(_realmId) gameActive canBuild;
```
**moveTile** Allows a parcel owner to move a tile to another coordinate.

```
function upgradeInstallation( uint256 _realmId, uint256 _prevInstallationId, uint256 _nextInstallationId,  uint256 _coordinateX, uint256 _coordinateY
  ) external onlyInstallationDiamond ;
```
**upgradeInstallation** removes previous installation and replaces it with a new one.

```
function addUpgradeQueueLength(uint256 _realmId) external onlyInstallationDiamond;
```
**addUpgradeQueueLength** increases the length of upgrades on queue 

```
function subUpgradeQueueLength(uint256 _realmId) external onlyInstallationDiamond;
```
**addUpgradeQueueLength** decreases the length of upgrades on queue 

```
function fixGrid( uint256 _realmId, uint256 _installationId, uint256[] memory _x, uint256[] memory _y, bool tile
  ) external onlyOwner;
```
**fixGrid**; the purpose of this function is to provide the contract owner with a way to modify the positions of tiles and installations within a particular realm, which can be useful for fixing issues or improving the overall gameplay experience for users.

```

function buildingFrozen() external view returns (bool);
```
**buildingFrozen** returns bool on the status of building
```
function setFreezeBuilding(bool _freezeBuilding) external onlyOwner;
```
**setFreezeBuilding** Allows diamond owner to set status of building whether true or false. 

```
function batchEquip(uint256 _realmId, uint256 _gotchiId, BatchEquipIO memory _params, bytes[] memory _signatures) external gameActive canBuild {
```
**batchEquip** enables parcel owners to equip and unequip installations and tiles at a go.