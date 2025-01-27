# ‼️ **Better use [Telethon](https://lonamiwebs.github.io/Telethon/)**

It does have docs


# api-mtproto

[![npm version][npm-image]][npm-url]

**Telegram Mobile Protocol** [(MTProto)](https://core.telegram.org/mtproto) library in **es6**

## About MTProto..

**MTProto** is the [Telegram Messenger](http://www.telegram.org) protocol
_"designed for access to a server API from applications running on mobile devices"_.

The Mobile Protocol is subdivided into three components ([from the official site](https://core.telegram.org/mtproto#general-description)):

 - High-level component (API query language): defines the method whereby API
 queries and responses are converted to binary messages.

 - Cryptographic (authorization) layer: defines the method by which messages
 are encrypted prior to being transmitted through the transport protocol.

 - Transport component: defines the method for the client and the server to transmit
 messages over some other existing network protocol (such as, http, https, tcp, udp).



## api-mtproto in short..

No more additional libs.
The **api-mtproto** library implements the **Mobile Protocol** and provides all features for work with telegram protocol:

 - A high level api for server connection

 - Promise-based API

 - **HTTP connections** implemented in the transport layer

 - **Web worker** support for blazing fast crypto math works in background

 - A cipher implementation for **AES and RSA encryption** in the security layer

 - Both **plain-text and encrypted message** to communicate data with the server

 - **Diffie-Hellman key exchange** supported by the **prime factorization** function implemented in the security layer

 - **MTProto TL-Schema** compilation as **javascript classes and functions**

 - Custom **async storage** support for saving user data between sessions


## Installation

```bash
$ npm install --save api-mtproto@beta
```

## Usage

```javascript
import MTProto from 'api-mtproto'

const phone = {
  num : '+9996620001',
  code: '22222'
}

const api = {
  layer          : 57,
  initConnection : 0x69796de9,
  api_id         : 49631
}

const server = {
  dev: true //We will connect to the test server.
}           //Any empty configurations fields can just not be specified

const client = MTProto({ server, api })

async function connect(){
  const { phone_code_hash } = await client('auth.sendCode', {
    phone_number  : phone.num,
    current_number: false,
    api_id        : 49631,
    api_hash      : 'fb050b8f6771e15bfda5df2409931569'
  })
  const { user } = await client('auth.signIn', {
    phone_number   : phone.num,
    phone_code_hash: phone_code_hash,
    phone_code     : phone.code
  })

  console.log('signed as ', user)
}

connect()
```

Above we used two functions from the API.
```typescript
type auth.sendCode = (phone_number: string, sms_type: int,
  api_id: int, api_hash: string, lang_code: string) => {
    phone_registered: boolean,
    phone_code_hash: string,
    send_call_timeout: int,
    is_password: boolean
  }

type auth.signIn = (phone_number: string, phone_code_hash: string, phone_code: string) => {
  expires: int,
  user: User
}
```
[More][send-code] about [them][sign-in], as well as about many other methods, you can read in the [official documentation][docs].

Additional examples can be obtained from [examples][examples] folder.

## Storage

You can use your own storages like [localForage][localForage] for saving data.
Module accepts the following interface

```typescript
interface AsyncStorage {
  get(key: string): Promise<any>;
  set(key: string, value: any): Promise<void>;
  remove(...keys: string[]): Promise<void>;
  clear(): Promise<void>;
}
```

```javascript
import { MTProto } from 'api-mtproto'
import { api } from './config'
import CustomStorage from './storage'

const client = MTProto({
  api,
  app: {
    storage: CustomStorage
  }
})

```

## License

The project is released under the [Mit License](./LICENSE)

[examples]: https://github.com/zerobias/api-mtproto/tree/develop/examples
[localForage]: https://github.com/localForage/localForage
[docs]: https://core.telegram.org/
[send-code]: https://core.telegram.org/method/auth.sendCode
[sign-in]: https://core.telegram.org/method/auth.signIn
[npm-url]: https://www.npmjs.org/package/api-mtproto
[npm-image]: https://badge.fury.io/js/api-mtproto.svg
