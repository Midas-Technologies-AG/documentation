# Pricing

## General

The Melon funds receive current pricing data from the Kyber Network, which in turn aggregates Kyber Reserve Managers providing markets for individual asset tokens. Asset prices are derived from the best price offered among participating reserve managers.

The PriceSource interface definition is provided below. KyberPriceFeed.sol implements the PriceSource.i.sol interface.
&nbsp;

## PriceSource.i.sol

#### Description

The PriceSource.i.sol is an interface definition intended to generalize the implementation of any concrete price source provider.

#### Interface Functions

`function getPriceInfo(address _asset) view returns (uint price, uint decimals)`

This public view function returns the asset token price and the asset token decimal precision given the `_asset` address parameter provided.
&nbsp;

`function getInvertedPriceInfo(address ofAsset) view returns (uint price, uint decimals)`

This public view function returns the asset token price provided by the `ofAsset` address parameter in terms of the `ofAsset` asset and decimal precision of the asset token address provided.  
&nbsp;

`function getQuoteAsset() public view returns (address)`

This public view function returns the address of asset token contract which was configured to be the base- or quote asset of the fund. The quote asset is the asset in which the fund's value is denominated.
&nbsp;

`function hasValidPrice(address) public view returns (bool)`

This public view function returns a boolean indicating whether the asset represented by the address parameter provided is valid. A return value of `true` indicates that the asset has a valid price. A return value of `false` indicates that the asset does not have a valid price.
&nbsp;

`function hasValidPrices(address[]) public view returns (bool)`

This public view function returns a boolean indicating whether all of the assets represented by the `address[]` array parameter provided are valid. A return value of `true` indicates that all asset tokens have valid prices. A return value of `false` indicates that one or more of the asset tokens do not have a valid price.
&nbsp;

`function getPrice(address _asset) public view returns (uint price, uint timestamp)`

This public view function returns the price and price timestamp of the asset token given by the asset token address provided. The timestamp represents the time of the price quote.
&nbsp;

`function getPrices(address[] _assets) public view returns (uint[] prices, uint[] timestamps)`

This public view function returns an array of prices and a corresponding array of price timestamps of the asset tokens given by the array of asset token addresses provided. The timestamps represent the times of the price quote.
&nbsp;

`function getReferencePriceInfo(address _base, address _quote) public view returns (uint referencePrice, uint decimal)`

This public view function takes two address parameters: the base asset (the object of the pricing information) and the quote asset (the asset in which the price is denominated). The function returns the asset price provided by the `ofBase` address parameter in terms of the quote asset, and the specified asset token's `decimals` property.
&nbsp;

`function getOrderPriceInfo(address sellAsset, address buyAsset, uint sellQuantity, uint buyQuantity) public view returns (uint orderPrice)`

This public view function returns a price given the provided sell asset and corresponding quantity, and the buy asset and corresponding quantity.
&nbsp;

`function existsPriceOnAssetPair(address sellAsset, address buyAsset) public view returns (bool isExistent)`

This public view function returns a boolean indicating whether a recent price exists for the asset pair provided by the `sellAsset` and `buyAsset` address parameters.
&nbsp;

## KyberPriceFeed.sol

#### Description

The KyberPriceFeed contract provides the interface from the version-specific Melon platform to the Kyber network for the purposes of receiving pricing data.

#### Inherits from

PriceSourceInterface, DSThing (link)

&nbsp;

#### On Construction

The `KyberPriceFeed` contract requires the following parameters on construction:

`address ofRegistrar` - The address of the Version's Registrar contract.
`address ofKyberNetworkProxy` - The address of the Kyber network proxy contract.
`uint ofMaxSpread` - The the maximum spread between bid- and ask prices for the price to be considered valid.
`address ofQuoteAsset` - The address of the asset token contract which is the fund's base- or quote asset. All asset prices will be denominated or based in this asset token across all funds for the purposes of pricing.

These parameters are used to set the the following public state variables on the contract:

`KYBER_NETWORK_PROXY`
`MAX_SPREAD`
`QUOTE_ASSET`
`REGISTRY`
&nbsp;

#### Structs

None.

&nbsp;

#### Enums

None.

&nbsp;

#### Modifiers

None.

&nbsp;


#### Events

None.

&nbsp;


#### Public State Variables

`address public KYBER_NETWORK_PROXY`

The address of the Kyber network proxy contract.
&nbsp;

`address public QUOTE_ASSET`

The address of the asset token contract which is the fund's base- or quote asset.
&nbsp;

`Registry public REGISTRY`

A public state variable which is the Registry contract of version under which the fund was deployed.
&nbsp;

`uint public MAX_SPREAD`

A public state variable which represents the maximum spread between derived bid- and ask prices for specific asset pairs. `MAX_SPREAD` represents the maximum acceptable tolerance for a price to be valid. `MAX_SPREAD` is specified as a percentage and formatted in 10^18 terms. For example, 1.0% (0.01) would be represented 1x10^16 or 10000000000000000.
&nbsp;

`address public constant KYBER_ETH_TOKEN = 0x00eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee`

A constant public state variable which represents the token contract address of the ETH asset token.
&nbsp;

`uint public constant KYBER_PRECISION = 18`

A constant public state variable which represents the divisibility precision of asset tokens...
&nbsp;

`uint public constant VALIDITY_INTERVAL = 2 days`

A constant public state variable which represents the maximum validity of price feed prices and is set to "2 days".

`uint public lastUpdate`

An integer representing the time of the last price feed update.
&nbsp;

`mapping (address => uint) public prices`

A public mapping associating an asset token contract address to the most recent price.
&nbsp;

#### Public Functions

`function update()`

This public function ensures that is can only be called by the `REGISTRY` owner. The function updates the `prices` mapping and the `lastUpdate` state variable. A price of "0" indicates an invalid price.
&nbsp;

`function getQuoteAsset() view returns (address)`

This public view function returns the address of asset token contract which was configured to be the base- or quote asset of the fund. The quote asset is the asset in which the fund's value is denominated.
&nbsp;

`function getPrice(address _asset) view returns (uint price, uint timestamp)`

This public view function returns the price and price timestamp of the asset token given by the asset token address provided. The price integer returned is formatted as the product of the exchange price and ten to the power of the asset's specified decimals property. The timestamp represents the time of the price quote.
&nbsp;

`function getPrices(address[] _assets) view returns (uint[], uint[])`

This public view function returns an array of prices and a corresponding array of price timestamps of the asset tokens given by the array of asset token addresses provided. The price integers returned are formatted as the product of the exchange price and ten to the power of the asset's specified decimals property. The timestamps represent the times of the price quote.
&nbsp;

`function hasValidPrice(address _asset) view returns (bool)`

This public view function returns a boolean indicating whether the asset represented by the `_asset` parameter provided is recent. The function first requires that the asset is registered in the registry. Passing the address of the quote asset will return `true`. If the price provided by the Kyber reserve manager is not 0, the price is assumed to be recent and will return `true`.
&nbsp;

`function hasValidPrices(address[] _assets) view returns (bool)`
This public view function returns a boolean indicating whether the assets represented by the `_assets` address array parameter provided are all recent. The function requires that all assets passed in address array `_assets` parameter are registered in the registry. If any single asset in the array of asset addresses is not recent, the function will return `false`.
&nbsp;

`function getPriceInfo(address ofAsset) view returns (bool isRecent, uint price, uint assetDecimals)`

This public view function calls the `getReferencePriceInfo()` function and returns a boolean indicating the validity of the price, the asset price provided by the `ofAsset` address parameter in terms of the quote asset, and the specified asset token's `decimals` property.
&nbsp;

`function getRawReferencePriceInfo(address _baseAsset, address _quoteAsset) view
returns (bool isValid, uint referencePrice, uint decimals)`

This public view function takes the `_baseAsset` and `_quoteAsset` token contract addresses and returns a boolean indicating price validity, the reference price and the asset token's decimal precision.
&nbsp;

`function getInvertedPriceInfo(address ofAsset) view returns (bool isRecent, uint invertedPrice, uint assetDecimals)`

This public view function calls the `getReferencePriceInfo()` function and returns a boolean indicating the validity of the price, the asset price provided by the `ofAsset` address parameter in terms of the `ofAsset` asset, and the specified asset token's `decimals` property.
&nbsp;

`function getReferencePriceInfo(address _baseAsset, address _quoteAsset) view returns (uint referencePrice, uint decimals)`

This public view function takes two address parameters: the base asset (the object of the pricing information) and the quote asset (the asset in which the price is denominated). [CHECK: function will we re-worked...]
&nbsp;

`function getOrderPriceInfo(address sellAsset, address buyAsset, uint sellQuantity, uint buyQuantity) view returns (uint orderPrice)`

This public view function returns a price given the provided sell asset and corresponding quantity, and the buy asset and corresponding quantity.
&nbsp;

`function existsPriceOnAssetPair(address sellAsset, address buyAsset) view returns (bool isExistent)`

This public view function returns a boolean indicating whether a recent price exists for the asset pair provided by the `sellAsset` and `buyAsset` address parameters.
&nbsp;

`function getKyberMaskAsset(address _asset) returns (address)`

This public function returns the address of the Kyber Network representation contract adddress of the asset token contract address provided. If the address provided is ETH, the native token, the function returns `KYBER_ETH_TOKEN`; otherwise `_asset` is returned.
&nbsp;

`function getKyberPrice(address _baseAsset, address _quoteAsset) public view
returns (bool isValid, uint kyberPrice)`

This public view function takes the `_baseAsset` and `_quoteAsset` token contract addresses and returns a boolean indicating price validity and the average expected current Kyber Network price.
&nbsp;

`function convertQuantity(uint fromAssetQuantity, address fromAsset, address toAsset)
public view returns (uint)`

This public view function calculates and returns the quantity of the `toAsset` token which has a current value equal to the `fromAssetQuantity` of the `fromAsset` token.
&nbsp;

`function getLastUpdate() public view returns (uint)`

This public view function returns the blocktime timestamp of the last price update, `lastUpdate`.
&nbsp;
