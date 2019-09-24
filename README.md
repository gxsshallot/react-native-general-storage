# react-native-general-storage

[![npm version](https://img.shields.io/npm/v/react-native-general-storage.svg?style=flat)](https://www.npmjs.com/package/react-native-general-storage)
[![Build Status](https://travis-ci.org/gaoxiaosong/react-native-general-storage.svg?branch=master)](https://travis-ci.org/gaoxiaosong/react-native-general-storage)

[中文说明](https://www.jianshu.com/p/43c2f39992b4)

It is the wrapper of `AsyncStorage` to support multi-level key and key prefix.

## Install

Install by Yarn:

```shell
yarn add react-native-general-storage
```

Install by NPM:

```shell
npm install --save react-native-general-storage
```
## note

  `import {AsyncStorage} from 'react-native'` will deprecated in RN Higher version. it move to [react-native-community/async-storage](https://github.com/react-native-community/async-storage)
  
   install dependencies see `react-native-community/async-storage` detail
   

## Usage

Import the module in the file:

```javascript
import AsyncStorage from 'react-native-general-storage';
```

### Keys Structure

You can use an array of string as a key. Or a string is also valid, and will be converted to an array.

The array of string means the levels of key, such as:

```javascript
key1 = ['App', 'UserInfo'];
key2 = ['App', 'UserSetting'];
```

### Prefix

You can set the common prefix to added into key automatically. In application, we usually used two prefix:

```javascript
const commonPart = '__common__';
const userPart = '__user__';

// When app start, we set the common prefix
AsyncStorage.setPrefix(commonPart, commonPart);

// When user login, we set user prefix as default prefix
const userId = '12345';
AsyncStorage.setPrefix(userPart, userId, true);

// When user logout, we unset user prefix and remove default prefix
AsyncStorage.setPrefix(userPart, null);
```

`setPrefix` function has three parameters:

* key: Key prefix used as identifier when you call interface.
* value: Prefix value used in the storage key. If it is null or undefined, the prefix will be deleted.
* isDefault: Set as default key, only valid when value is not null or undefined.

### Seperator

Default seperator used in storage key. The default value is `'$'`. You can modify it globally:

```javascript
AsyncStorage.defaultSeperator = '#';
```

### Storage Key

The key stored in React Native `AsyncStorage` is composed by prefix, keys and seperator:

```javascript
storageKey = [...prefix, ...keys].join(seperator);
```

### Interface

* set: `(keys, content, prefix) => Promise<void>`
* get: `(keys, prefix) => Promise<object>`
* remove: `(keys, prefix) => Promise<void>`
* merge: `(keys, content, prefix) => Promise<void>`
* clear: `(keys, prefix) => Promise<void>`
* getKeys: `(keys, prefix) => Promise<{string: object}>`
* multiGet: `(keys, prefix) => Promise<object[]>`
* multiSet: `(keys, values, prefix) => Promise<void>`
* multiRemove: `(keys, prefix) => Promise<void>`

Parameters:

* `keys` is a string or an array of string, you can see `Keys Structure`.
* `prefix` is prefix key. Use default prefix if it is undefined.
* `content` is an object, an array, a string or a number.

**clear** and **getKeys**:

They are manipulate a set of keys, which has same prefix.

If one key is `['App', 'UserInfo']` and another is `['App', 'UserSetting']`, and both is `'userPart'` prefix. When `clear(['App'], userPart)`, they will be clear. Or `getKeys(['App'], userPart)`, they will be returned in promise.
