# react-native-general-storage

这是`AsyncStorage`的封装，用以支持多级键列表以及公共前缀。

## 安装

使用Yarn安装:

```shell
yarn add react-native-general-storage
```

使用NPM安装:

```shell
npm install --save react-native-general-storage
```

## 使用

在文件中引入模块:

```javascript
import AsyncStorage from 'react-native-general-storage';
```

### 键列表结构

可以使用一个字符串数组作为键列表, 或者一个字符串也可以, 会被自动转换成数组.

字符串数组意味着键的层级结构关系, 例如:

```javascript
key1 = ['App', 'UserInfo'];
key2 = ['App', 'UserSetting'];
```

### 前缀

你可以设置公共前缀, 会被自动加到存储键中. 在应用中, 我们通常使用两种前缀:

```javascript
const commonPart = '__common__';
const userPart = '__user__';

// 当应用启动后, 我们设置common前缀
AsyncStorage.setPrefix(commonPart, commonPart);

// 当用户登陆后, 我们把用户Id设置为user前缀, 并设置user前缀为默认前缀
const userId = '12345';
AsyncStorage.setPrefix(userPart, userId, true);

// 当用户登出后, 我们删除user前缀, 并移除默认前缀的设置
AsyncStorage.setPrefix(userPart, null);
```

`setPrefix`方法有三个参数:

* key: 前缀的键标识, 用于调用接口时的传递参数.
* value: 前缀实际的值, 用于存储键的组成. 如果是null或undefined, 则该前缀会被删除.
* isDefault: 是否是默认前缀, 只有当value不是null或undefined时候生效.

### 分隔符

在存储键中使用的默认分隔符. 默认值是`'$'`. 可以全局的修改它:

```javascript
AsyncStorage.defaultSeperator = '#';
```

### 存储键

在React Native的`AsyncStorage`中存储的键, 是使用前缀、键列表和分隔符组成的:

```javascript
storageKey = [...prefix, ...keys].join(seperator);
```

### 接口

* set: `(keys, content, prefix = undefined) => Promise<void>`
* get: `(keys, prefix = undefined) => Promise<object>`
* remove: `(keys, prefix = undefined) => Promise<void>`
* merge: `(keys, content, prefix = undefined) => Promise<void>`
* clear: `(keys, prefix = undefined) => Promise<void>`
* getKeys: `(keys, prefix = undefined) => Promise<{string: object}>`

参数说明:

* `keys`是一个字符串或者字符串数组, 可以参照`键列表结构`部分.
* `prefix`是前缀的标识符. 如果是undefined, 则使用默认前缀.
* `content`是一个对象、数组、字符串或数字.

**clear**和**getKeys**:

它们都是同时操作一组拥有同样'前缀'的键.

如果一个键是`['App', 'UserInfo']`, 另一个是`['App', 'UserSetting']`, 并且二者都是`'userPart'`前缀. 当`clear(['App'], userPart)`调用时, 它们都会被删除. 或者`getKeys(['App'], userPart)`调用时, 它们都会在Promise中被返回.