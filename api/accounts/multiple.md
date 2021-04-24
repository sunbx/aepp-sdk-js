<a id="module_@aeternity/aepp-sdk/es/accounts/multiple"></a>

## @aeternity/aepp-sdk/es/accounts/multiple
AccountMultiple module


* [@aeternity/aepp-sdk/es/accounts/multiple](#module_@aeternity/aepp-sdk/es/accounts/multiple)
    * [module.exports([options])](#exp_module_@aeternity/aepp-sdk/es/accounts/multiple--module.exports) ⇒ `Object` ⏏
    * [addresses()](#exp_module_@aeternity/aepp-sdk/es/accounts/multiple--addresses) ⇒ `Array.&lt;String&gt;` ⏏
    * [removeAccount(address)](#exp_module_@aeternity/aepp-sdk/es/accounts/multiple--removeAccount) ⇒ `void` ⏏
    * _async_
        * [addAccount(account, [options])](#exp_module_@aeternity/aepp-sdk/es/accounts/multiple--addAccount) ⇒ `void` ⏏

<a id="exp_module_@aeternity/aepp-sdk/es/accounts/multiple--module.exports"></a>

### module.exports([options]) ⇒ `Object` ⏏
AccountMultiple stamp

The purpose of this stamp is to wrap up implementations of
[module:@aeternity/aepp-sdk/es/account/base--AccountBase](module:@aeternity/aepp-sdk/es/account/base--AccountBase) objects and provide a
common interface to all of them. List are a substantial part of
[module:@aeternity/aepp-sdk/es/ae/wallet--Wallet](module:@aeternity/aepp-sdk/es/ae/wallet--Wallet)s.

**Kind**: Exported function  
**Returns**: `Object` - AccountMultiple instance  
**rtype**: `Stamp`

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| [options] | `Object` | <code>{}</code> | Initializer object |
| [options.accounts] | `Array` |  | Accounts array |
| [options.address] | `String` |  | Address of account to select |

**Example**  
```js
const accounts = await AccountMultiple({ accounts: [ MemmoryAccount({ keypair: 'keypair_object' }) ] })
await accounts.addAccount(account, { select: true }) // Add account and make it selected
accounts.removeAccount(address) // Remove account
accounts.selectAccount(address) // Select account
accounts.addresses() // Get available accounts
```
<a id="exp_module_@aeternity/aepp-sdk/es/accounts/multiple--addresses"></a>

### addresses() ⇒ `Array.&lt;String&gt;` ⏏
Get accounts addresses

**Kind**: Exported function  
**rtype**: `() => String[]`
**Example**  
```js
addresses()
```
<a id="exp_module_@aeternity/aepp-sdk/es/accounts/multiple--removeAccount"></a>

### removeAccount(address) ⇒ `void` ⏏
Remove specific account

**Kind**: Exported function  
**rtype**: `(address: String) => void`

| Param | Type | Description |
| --- | --- | --- |
| address | `String` | Address of account to remove |

**Example**  
```js
removeAccount(address)
```
<a id="exp_module_@aeternity/aepp-sdk/es/accounts/multiple--addAccount"></a>

### addAccount(account, [options]) ⇒ `void` ⏏
Add specific account

**Kind**: Exported function  
**Category**: async  
**rtype**: `(account: Account, { select: Boolean }) => void`

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| account | `Object` |  | Account instance |
| [options] | `Object` | <code>{}</code> | Options |
| [options.select] | `Boolean` |  | Select account |

**Example**  
```js
addAccount(account)
```
