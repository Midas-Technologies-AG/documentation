# Registry

## General

The Registry contract is a singleton contract for all Version contracts. Each specific Melon protocol Version deploys funds generated of that specific version.
&nbsp;

## Registry.sol

#### Description

The Registry contract stores, manages and provides functionality to maintain all relevant data pertaining to individual asset tokens and individual exchanges eligible for investment and operation within the Melon Protocol. The contract provides functionality to define, add, update or remove asset tokens and exchanges.
&nbsp;

#### Inherits from

DSAuth (link)
&nbsp;

#### On Construction

None.
&nbsp;

#### Structs

`Asset`

  Member variables:  

  `bool exists` - A boolean to conveniently and clearly indicate existence in a mapping.
  `string name` - The human-readable name of the asset token.
  `string symbol` - The human-readable symbol of the asset token.
  `uint decimals` -  The divisibility precision of the token.
  `string url` - The URL for extended information of the asset token.
  `uint reserveMin` - An integer representing the Kyber Network reserve minimum.
  `uint[] standards` - An array of integers representing EIP standards to which this asset token conforms.
  `bytes4[] sigs` - An array of asset token function signatures which have been whitelisted.


This struct stores all relevant information pertaining to the asset token.
&nbsp;

`Exchange`

  Member variables:  

  `bool exists` - A boolean to conveniently and clearly indicate existence in a mapping.
  `address adapter` - The address of the exchange's corresponding adapter contract registered.
  `bool takesCustody` - A boolean indicating the custodial nature of the exchange.
  `bytes4[] sigs` - An array of exchange function signatures which have been whitelisted.

This struct stores all relevant information pertaining to the exchange.
&nbsp;

Version

  Member variables:  

  `bool exists` -  A boolean to conveniently and clearly indicate existence in a mapping.
  `bytes32 name` - A 32 byte value type to represent the name of the Version.

This struct stores all relevant information pertaining to the Version.
&nbsp;

#### Enums

None.

&nbsp;

#### Modifiers

`modifier only_version()`

A modifier which requires `msg.sender` be a Version contract.
&nbsp;


#### Events

`AssetUpsert()`

  `address indexed asset` - The address of the asset token contract registered.
  `string name` - The human-readable name of the asset token.
  `string symbol` - The human-readable symbol of the asset token.
  `uint decimals` - The divisibility precision of the token.
  `string url` - The URL for extended information of the asset token.
  `uint reserveMin` - An integer representing the Kyber Network reserve minimum.
  `uint[] standards` - An array of integers representing EIP standards to which this asset token conforms.
  `bytes4[] sigs` - An array of asset token function signatures which have been whitelisted.

This event is triggered when an asset is added or updated in `registeredAssets`. The parameters listed above are provided with the event.
&nbsp;

`event ExchangeUpsert()`

  `address indexed exchange` - The address of the exchange contract registered.
  `address indexed adapter` - The address of the exchange's corresponding adapter contract registered.
  `bool takesCustody` - A boolean indicating the custodial nature of the exchange.
  `bytes4[] sigs` - An array of exchange function signatures which have been whitelisted.

This event is triggered when an exchange is added or updated in `registeredExchanges`. The parameters listed above are provided with the event.
&nbsp;

`event AssetRemoval()`

  `address indexed asset` - The address of the registered asset token contract to be removed.

This event is triggered when an asset is removed from the `registeredAssets` array state variable. The parameter listed above are provided with the event.
&nbsp;

`event ExchangeRemoval()`

  `address indexed exchange` - The address of the registered exchange contract to be removed.

This event is triggered when an exchange is removed from the `registeredExchanges` array state variable. The parameter listed above are provided with the event.
&nbsp;

`event VersionRegistration()`

  `address indexed version` - The address of the Version registered.

This event is triggered when a Version is registered. The parameter listed above are provided with the event.
&nbsp;

`event PriceSourceChange()`

  `address indexed priceSource` - The address of the new price source.

This event is triggered when a price source is changed. The parameter listed above are provided with the event.
&nbsp;

`event MlnTokenChange()`

  `address indexed mlnToken` - The address of the new MLN token contract.

This event is triggered when the MLN token is migrated to a new contract. The parameter listed above are provided with the event.
&nbsp;

`event NativeAssetChange()`

  `address indexed nativeAsset` - The address of the new native asset token contract.

This event is triggered when the native asset token is migrated to a new contract. The parameter listed above are provided with the event.
&nbsp;

`event EngineChange()`

  `address indexed engine` - The address of the new Melon Engine contract.

This event is triggered when the Melon Engine is migrated to a new contract. The parameter listed above are provided with the event.
&nbsp;

#### Public State Variables

`mapping (address => Asset) public assetInformation`

This public state variable mapping maps an asset token contract `address` to an `Asset` strut containing the asset information described above.
&nbsp;

`address[] public registeredAssets`

This public state variable is an array of addresses which stores each asset token contract `address` which is registered.
&nbsp;

`mapping (address => Exchange) public exchangeInformation`

This public state variable mapping maps an exchange contract `address` to an `Exchange` strut containing the exchange information described above.
&nbsp;

`address[] public registeredExchangeAdapters`

This public state variable is an array of addresses which stores each exchange adapter contract `address` which is registered.
&nbsp;

`mapping (address => Version) public versionInformation`

This public state variable mapping maps a Version contract `address` to a `Version` strut containing the Version information described above.
&nbsp;

`address[] public registeredVersions`

This public state variable is an array of addresses which stores each Version contract `address` which is registered.
&nbsp;

`mapping (address => bool) public isFeeRegistered`

This public state variable mapping maps a Fee contract `address` to a boolean indicating that the Fee contract is registered.
&nbsp;

`mapping (address => address) public fundsToVersions`

This public state variable mapping maps a Version contract `address` to a Melon fund address.
&nbsp;

`mapping (bytes32 => bool) public versionNameExists`

This public state variable mapping maps the Version name to a boolean indicating the Version name existence.
&nbsp;

`mapping (bytes32 => address) public fundNameHashToOwner`

This public state variable mapping maps the hash of the fund name to the fund owner. A `bytes32` type is used, as dynamic-length types cannot be used as keys in mappings and a hash of the fund name allows for names longer than 32 bytes.
&nbsp;

`uint public constant MAX_REGISTERED_ENTITIES = 20`

This public constant state variable represents the maximum quantity of registered entities and is set to "20". This constant applies to Exchanges registered and Assets registered.
&nbsp;

`uint public constant MAX_FUND_NAME_BYTES = 66`

This public constant state variable represents the maximum Melon fund name size in bytes. The maximum is set to 66 bytes.
&nbsp;

`address public priceSource`

This public state variable is the address of the current, active price source contract.
&nbsp;

`address public mlnToken`

This public state variable is the address of the Melon token contract.
&nbsp;

`address public nativeAsset`

This public state variable is the address of the native asset token contract.
&nbsp;

`address public engine`

This public state variable is the address of the Melon Engine contract.
&nbsp;

`address public ethfinexWrapperRegistry`  

This public state variable is the address of the Ethfinex Wrapper Registry contract.
&nbsp;

`uint public incentive = 10 finney`

This public state variable defines the incentive amount in ETH. The variable is set to 10 finney.
&nbsp;

#### Public Functions

`function registerAsset(
        address _asset,
        string _name,
        string _symbol,
        string _url,
        uint _reserveMin,
        uint[] _standards,
        bytes4[] _sigs
    ) external auth`

This external function requires that the caller is the `owner` or the current contract. It then requires that the asset token's information not be previously registered. The asset token's address is then appended to the `registeredAssets` array state variable. The function then registers the following information for a specific asset token within the Registry contract as per the following parameters:

`address _asset` - The address of the asset token contract to be registered.
`string _name` - The human-readable name of the asset token.
`string _symbol` - The human-readable symbol of the asset token.
`string _url` - The URL for extended information of the asset token.
`uint reserveMin` - An integer representing the Kyber Network reserve minimum.
`uint[] _standards` - An array of integers representing EIP standards to which this asset token conforms.
`bytes4[] _sigs` - An array of asset token function signatures which have been whitelisted.

Finally, the function ensures that the asset token's information exists in the `assetInformation` mapping state variable.
&nbsp;

`function registerExchangeAdapter(
    address _exchange,
    address _adapter,
    bool _takesCustody,
    bytes4[] _sigs
) external auth`

This external function requires that the caller is the `owner` or the current contract. It then requires that the exchange's information not be previously registered. The exchange's address is then appended to the `registeredExchanges` array state variable. The function then registers the following information for a specific exchange within the Registry contract as per the following parameters:

`address _exchange` - The address of the exchange contract to be registered.
`address _adapter` - The address of the exchange's corresponding adapter contract to be registered.
`bool _takesCustody` - A boolean indicating the custodial nature of the exchange.
`bytes4[] _sigs` - An array of exchange function signatures which have been whitelisted.

Finally, the function ensures that the exchange's information exists in the `exchangeInformation` mapping state variable.
&nbsp;

`function updateAsset(
    address _asset,
    string _name,
    string _symbol,
    uint _decimals,
    string _url,
    uint _reserveMin,
    uint[] _standards,
    bytes4[] _sigs
) auth`

This function requires that the caller is the `owner` or the current contract. It then requires that the asset token's information be previously registered. The function then updates the following information for a specific asset token within the Registry contract as per the following parameters:

`address _asset` - The address of the asset token contract to be registered.
`string _name` - The human-readable name of the asset token.
`string _symbol` - The human-readable symbol of the asset token.
`uint _decimals` - The divisibility precision of the token.
`string _url` - The URL for extended information of the asset token.
`uint reserveMin` - An integer representing the Kyber Network reserve minimum.
`uint[] _standards` - An array of integers representing EIP standards to which this asset token conforms.
`bytes4[] _sigs` - An array of asset token function signatures which have been whitelisted.

Finally, the function emits the `AssetUpsert` event along with the parameters listed above.
&nbsp;

`function updateExchange(
    address _exchange,
    address _adapter,
    bool _takesCustody,
    bytes4[] _sigs
) auth`

This function requires that the caller is the `owner` or the current contract. It then requires that the exchange's information be previously registered. The function then updates the following information for a specific exchange within the Registry contract as per the following parameters:

`address _exchange` - The address of the exchange contract to be registered.
`address _adapter` - The address of the exchange's corresponding adapter contract to be registered.
`bool _takesCustody` - A boolean indicating the custodial nature of the exchange.
`bytes4[] _sigs` - An array of exchange function signatures which have been whitelisted.

Finally, the function emits the `ExchangeUpsert` event along with the parameters listed above.
&nbsp;


`function removeAsset(address _asset, uint _assetIndex) auth`

This function requires that the caller is the `owner` or the current contract. The function requires the following parameters:

`address _asset` - The address of the asset token contract to be removed.
`uint _assetIndex` - The index of the asset provided in the registerAssets array state variable.

The function then requires that the asset information exists and that the asset is registered. The function then deletes the asset's entries from the `assetInformation` mapping state variable and the `registeredAssets` array state variable, while also maintaining the `registeredAssets` array. The function ensures that the asset's information entry no longer exists and finally emits the `AssetRemoval()` event with the asset's token contract address.
&nbsp;

`function removeExchange(address _exchange, uint _exchangeIndex) auth`

This function requires that the caller is the `owner` or the current contract. The function requires the following parameters:

`address _exchange` - The address of the exchange contract to be removed.
`uint _exchangeIndex` - The index of the exchange provided in the registerAssets array state variable.

The function then requires that the exchange information exists and that the exchange is registered. The function then deletes the exchange's entries from the `exchangeInformation` mapping state variable and the `registeredExchanges` array state variable, while also maintaining the `registeredExchanges` array. The function ensures that the exchange's information entry no longer exists and finally emits the `ExchangeRemoval()` event with the exchange's contract address.
&nbsp;

`function getName(address _asset) view returns (string)`

This public view function retrieves the asset token `name` for the registered asset address provided.
&nbsp;

`function getSymbol(address _asset) view returns (string)`

This public view function retrieves the asset token `symbol` for the registered asset address provided.
&nbsp;

`function getDecimals(address _asset) view returns (uint)`

This public view function retrieves the asset token `decimals` for the registered asset address provided. `decimals` specifies the divisibility precision of the token.
&nbsp;

`function assetIsRegistered(address _asset) view returns (bool)`

This public view function indicates whether the address provided is that of a registered asset token. A return value of `true` indicates that the asset is registered. A return value of `false` indicates that the asset is not registered.
&nbsp;

`function getRegisteredAssets() view returns (address[])`

This public view function returns an array of all registered asset token addresses.
&nbsp;

`function assetMethodIsAllowed(address _asset, bytes4 _sig) external view returns (bool)`

This external view function returns a boolean indicating whether a specific asset token function is whitelisted given the provided asset token address and the function's signature hash. A return value of `true` indicates that the function is whitelisted. A return value of `false` indicates that the function is not whitelisted.
&nbsp;

`function exchangeIsRegistered(address _exchange) view returns (bool)`

This public view function returns a boolean indicating whether a specific exchange is registered. A return value of `true` indicates that the exchange is registered. A return value of `false` indicates that the exchange is not registered.
&nbsp;

`function getRegisteredExchanges() view returns (address[])`

This public view function returns an array of all registered exchange addresses.
&nbsp;

`function getExchangeInformation(address _exchange) view returns (address, bool)`

This public view function returns the corresponding exchange adapter contract address and the exchange's boolean indicator `takesCustody` given the provided exchange contract address.
&nbsp;

`function getExchangeFunctionSignatures(address _exchange) view returns (bytes4[])`

This public view function returns an array of whitelisted exchange function signatures corresponding to the provided exchange contract address.
&nbsp;

`function exchangeMethodIsAllowed(address _exchange, bytes4 _sig) returns (bool)`

This public view function returns a boolean indicating whether a specific exchange function is whitelisted given the provided exchange address and the function's signature hash. A return value of `true` indicates that the function is whitelisted. A return value of `false` indicates that the function is not whitelisted.
&nbsp;

`function registerVersion(address _version, bytes32 _name) auth`

This function requires that the caller is the `owner` or the current contract. The function sets the `versionInformation` mapping `exists` to `true`, `name` to `_name` and pushes the Version address on to the `registeredVersions` array. The `versionNameExists` mapping for this Version name is set to `true`. Finally, the `VersionRegistration()` event is emitted, logging the Version address. Versions cannot be removed from the registry.
&nbsp;

`function setPriceSource(address _priceSource) auth`

This function requires that the caller is the `owner` or the current contract. The function sets the `priceSource` state variable to the `_priceSource` parameter value and emits the `PriceSourceChange()` event, logging `_priceSource`.
&nbsp;

`function setMlnToken(address _mlnToken) auth`

This function requires that the caller is the `owner` or the current contract. The function sets the `mlnToken` state variable to the `_mlnToken` parameter value and emits the `MlnTokenChange()` event, logging `_mlnToken`.
&nbsp;

`function setNativeAsset(address _nativeAsset) auth`

This function requires that the caller is the `owner` or the current contract. The function sets the `nativeAsset` state variable to the `_nativeAsset` parameter value and emits the `NativeAssetChange()` event, logging `_nativeAsset`.
&nbsp;

`function setEngine(address _engine) auth`

This function requires that the caller is the `owner` or the current contract. The function sets the `engine` state variable to the `_engine` parameter value and emits the `EngineChange()` event, logging `_engine`.
&nbsp;

`function getReserveMin(address _asset) view returns (uint)`

This public view function returns the `reserveMin` for the asset token contract address provided.
&nbsp;

`function adapterForExchange(address _exchange) view returns (address)`

This public view function returns the address of the exchange adapter contract given the exchange contract address provided.
&nbsp;

`function getRegisteredVersions() view returns (address[])`

This public view function returns an exhaustive array of addresses of all registered Version contracts.
&nbsp;

`function isFund(address _who) view returns (bool)`

This public view function returns a boolean indicating whether the address provided is a Melon fund contract.
&nbsp;

`function isFundFactory(address _who) view returns (bool)`

This public view function returns a boolean indicating whether the address provided is a FundFactory. The function check the existence of the `_who` address in the `versionInformation` mapping. Note that Version inherits FundFactory.
&nbsp;

`function registerFund(address _fund, address _owner, string _name) external only_version`

This external function ensures that `msg.sender` is a Version, as only Versions can register funds. The function requires that the fund name is valid. The function then adds an entry to the `fundsToVersions` mapping, associating `_fund` to `msg.sender`, i.e. the Version address.
&nbsp;

`function isValidFundName(string _name) public view returns (bool)`

This public function ensures that the fund name provided does not exceed `MAX_FUND_NAME_BYTES` nor contain restricted characters. Legal characters are "0-9", "a-z", "A-Z", " ", "-", ".", "\_" and "\*".
&nbsp;

`function canUseFundName(address _user, string _name) public view returns (bool)`

This public function ensures that the fund name provided adheres to the rules set forth in `isValidFundName()`, and that the name is not already in use by a fund or is used by the provided `_user` address (to enable fund name reuse across Versions by the same `_user`).
&nbsp;

`function reserveFundName(address _owner, string _name) external only_version`

This external function requires that that `msg.sender` is a Version and that `_name` passes all conditions of `canUseFundName()`, then adds `_name` to the `fundNameHashToOwner` mapping.
&nbsp;

`function registerFees(address[] _fees) external auth`

This external function requires that the caller is the `owner` or the current contract. The Fee contract addresses provided are then added as entries to the `isFeeRegistered` mapping with the value of `true`.
&nbsp;

`function deregisterFees(address[] _fees) external auth`

This external function requires that the caller is the `owner` or the current contract. The Fee contract addresses provided are then deleted from the  `isFeeRegistered` mapping.
&nbsp;
