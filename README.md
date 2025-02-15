# Fractional contracts

## Contract (Goerli)

- SETTINGS : 0xbf8Cf127eEf456213ff328516AD53B27e97a7C7c (Owner : 0x1B394672A79de671E40068210D9d2FBc003e697e, Abito)
- TOKENVAULT : 0xAAD046D1A6Be69D8029121d9Ba3832AF1076624a
- TOKENVAULTFACTORY (TVF) : 0xEaAe04bC2EF3cc65Ec653BD55c1DFcB991Cf7582
- TEST NFT : 0x317a8Fe0f1C7102e7674aB231441E485c64c178A, https://testnets.opensea.io/assets/goerli/0x317a8fe0f1c7102e7674ab231441e485c64c178a/367145
- MyToken (MT) : 0x949DD2cE8Da448179454aD75Fb6AAE3725874295
- RESULT-ERC20 : 0x55EB8fB4F9A9D16f2740C24DCc39A6d11232094F

MyToken Contract가 TVF 주소를 approve해야 TVF가 MyToken을 나눌 수 있었다. 그런데 내게 필요한 것은 이런 MT 컨트랙트의 오너로부터 approve가 필요하지 않아야 한다. 오직 소유주가 TVF에 NFT를 전송한다면, 1.0개 토큰을 발행할 수 있어야한다.


## Settings
This is a generic settings contract to be owned by governance. It is gated by some high and low boundary values. It allows for governance to set parameters for all Token Vaults.

## Vault Factory
This is a simple factory where a user can call `mint` to create a new vault with the token that they want to fractionalize. The user must pass in:
- an ERC20 token desired name
- an ERC20 token desired symbol
- the ERC721 token address they wish to fractionalize
- the ERC721 token id they wish to fractionalize
- the desired ERC20 token supply
- the initial listing price of the NFT
- their desired curator fee

## Initialized Proxy
A minimal proxy contract to represent vaults which allows for cheap deployments.

## Token Vault
The token vault is the smart contract which holds the logic for NFTs that have been fractionalized.

Token owners are able to update the reserve price with a weighted average of all token owners reserve prices. This is done automatically on token transfer to new accounts or manually through `updateUserPrice`.

Alongside this logic, is auction logic which allows for an outside user to initial a buyout auction of the NFT. Here there are `start`, `bid`, `end` and `cash` functions.
#### Start
The function called to kick off an auction. `msg.value` must be greater than or equal to the current reserve price.
#### Bid
The function called for all subsequent auction bids.
#### End
The function called when the auction timer is up and ended.
#### Cash
The function called for token owners to cash out their share of the ETH used to purchase the underlying NFT.

There is also some admin logic for the `curator` (user who initially deposited the NFT). They can reduce their fee or change the auction length. Alongside this, they are able to claim fees in the form of token supply inflation.

## IndexERC721
This is a single token ERC721 which is used to custody multiple ERC721 tokens. 
#### depositERC721
Anyone can deposit an ERC721 token into the contract
#### withdrawERC721
The token owner can withdraw any ERC721 tokens in the contract
#### withdrawETH
The token owner can withdraw any ETH in the contract
#### withdrawERC20
The token owner can withdraw any ERC20 token in the contract

## Deployments
### Mainnet
[Vault Factory](https://etherscan.io/address/0x85aa7f78bdb2de8f3e0c0010d99ad5853ffcfc63)

[Token Vault](https://etherscan.io/address/0x7b0fce54574d9746414d11367f54c9ab94e53dca)

[Settings](https://etherscan.io/address/0xE0FC79183a22106229B84ECDd55cA017A07eddCa)

[Index ERC721 Factory](https://etherscan.io/address/0xde771104c0c44123d22d39bb716339cd0c3333a1)

### Rinkeby
[Vault Factory](https://rinkeby.etherscan.io/address/0x458556c097251f52ca89cB81316B4113aC734BD1)

[Token Vault](https://rinkeby.etherscan.io/address/0x825f25f908db46daEA42bd536d25f8633667f62b)

[Settings](https://rinkeby.etherscan.io/address/0x1C0857f8642D704ecB213A752A3f68E51913A779)

[Index ERC721 Factory](https://rinkeby.etherscan.io/address/0xee727b734aC43fc391b67caFd18e5DD4Dc939668)

