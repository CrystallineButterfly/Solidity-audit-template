C4 finding submitted:
risk = G (Gas Optimization)

Gas Optimization report by K42@crystallineButterfly

# 1: Gas optimization suggestions for ProfilePicture.sol

A. Instead of using a separate numMinted variable to track the number of tokens minted, you can use the
totalSupply() function provided by the ERC721 standard. This will save gas by reducing the number of storage
reads and writes.

On line = https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-pfp-
protocol/src/ProfilePicture.sol#L29

What improvement would look like below:

Remove the numMinted variable from the contract and update the mint function to this =
// Use totalSupply() instead of numMinted

`function mint(address \_nftContract, uint256 \_nftID) external {
uint256 tokenId = totalSupply() + 1;

if (ERC721(\_nftContract).ownerOf(\_nftID) != msg.sender)
revert PFPNotOwnedByCaller(msg.sender, \_nftContract, \_nftID);
ProfilePictureData storage pictureData = pfp[tokenId];
pictureData.nftContract = \_nftContract;
pictureData.nftID = \_nftID;
\_mint(msg.sender, tokenId);
emit PfpAdded(msg.sender, tokenId, \_nftContract, \_nftID);
}`

B. Instead of using a separate mapping for pfp, consider using ERC721 metadata standards to store this information in the token URI. This will save gas and simplify the contract.

On line = https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-pfp-protocol/src/ProfilePicture.sol#L32

# 2: Gas optimization suggestions for Bio.sol

A. Same as previous contract = Instead of using a separate numMinted variable to track the number of tokens minted, you can use the totalSupply() function provided by the ERC721 standard. This will save gas by reducing the number of storage reads and writes.

On line = https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-bio-protocol/src/Bio.sol#L15

B. The tokenURI function generates an SVG image on-chain, which could be gas-intensive, especially for longer bio texts. Consider storing the SVG image off-chain (e.g., on IPFS) and only storing the content hash in the contract. This would reduce the gas cost for generating the tokenURI.

On line = https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-bio-protocol/src/Bio.sol#L43

# 3: Gas optimization suggestions for Namespace.sol

A. Use the OpenZeppelin library: Instead of creating your own implementation of ERC721 and Owned, consider using OpenZeppelin's library, which is well-audited and optimized for gas usage.

B. Use 'unchecked' keyword: In loops where you are sure that integer overflow/underflow cannot occur, consider
using the 'unchecked' keyword to avoid extra gas costs related to SafeMath checks.

C. Optimize storage variables: Reorganize storage variables to make them more efficient by grouping smaller
variables together. This will help reduce gas costs associated with storage updates. E.g: Consider
combining the 'trayID' and 'tileOffset' into a single uint256.
On line = https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L30

Change to = `struct CharacterData {
/// @notice Combined TrayID and TileOffset (trayID << 8) | tileOffset
uint256 trayIDAndOffset;
/// @notice Emoji modifier for the skin tone. Can have values of 0 (yellow) and 1 - 5 (light to dark).
Only supported by some emojis
uint8 skinToneModifier;
}`

D. Reduce the number of events: Emitting events consumes gas. You can consider reducing the number of events emitted, such as merging the 'NamespaceFused' event with the ERC721 'Transfer' event, and only emitting the name information when necessary.

E. Use 'staticcall' for view functions: When calling view functions like 'tray.getTile', consider using 'staticcall' to reduce gas consumption. This can be done using the 'functionCall' function from OpenZeppelin's Address library.
On line = https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L133

# 4: Gas optimizations suggestions for Tray.sol

A. Use 'unchecked' blocks: In functions like buy, you can use 'unchecked' blocks to skip overflow/underflow checks for some arithmetic operations when you know they won't cause overflows/underflows. This can save gas.
On line = https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Tray.sol#L150

B. Combine storage variables: As suggested in a previous optimizations, consider combining the 'trayID' and 'tileOffset' into a single uint256 to reduce gas costs associated with storage updates.

C. Struct packing: Make sure that the struct elements are optimally ordered to save storage costs. In the TileData struct, you can reorder elements in decreasing size order: uint16, uint8, uint8. This will pack the struct more efficiently.
On line = https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Tray.sol#L57

D. Use 'staticcall' for view functions: Using 'staticcall' instead of 'delegatecall' when calling external view functions (like tokenURI) can save gas, as 'staticcall' does not allow state changes.

E. Cache frequently used storage variables: In some cases, caching storage variables in memory can help reduce gas costs associated with SLOAD operations. For example, in the buy function, you can cache the prelaunchMinted storage variable in memory.
On line = https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Tray.sol#L73

F. Optimize loop iterations: If possible, try to reduce the number of loop iterations or replace them with more efficient operations. For example, in the tokenURI function, you can use the abi.encode function to encode the entire storedNftTiles array instead of iterating through it.

An example of this =
`function tokenURI(uint256 \_tokenId) public view override returns (string memory) {
require(\_exists(\_tokenId), "ERC721Metadata: URI query for nonexistent token");

// Get the entire array of tiles without iterating through it
TileData[TILES_PER_TRAY] memory storedNftTiles = tiles[_tokenId];

// Encode the entire array at once using abi.encode
bytes memory encodedTiles = abi.encode(storedNftTiles);

// You can now create the final tokenURI using the encodedTiles, either by concatenating with a baseURI or any other method
string memory baseURI = \_baseURI();
return bytes(baseURI).length > 0 ? string(abi.encodePacked(baseURI, encodedTiles)) : '';
}`