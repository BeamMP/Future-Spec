## Login

### Purposes

```
0x0004  AskForCredentials // this is only for the initial login, if there's no stored key
0x0005  Credentials
0x0006  LoginResult
0xaa02  State change to QuickJoin
```

### Formats

#### AskForCredentials

Empty body.

#### Credentials

```json5
{
  "username": string,
  "password": string,
  "remember": true | false,
}
```

If `remember` is `false`, the launcher must NOT store the new private key. If it's `true`, the launcher must store the private key in the filesystem, and use the `/userlogin` endpoint with `{"pk": <key>}` to log in when re-started.

#### LoginResult

```json5
{
  "success": true | false,
  "message": string,
  // null on error
  "username": string,
  // null on error
  "role": string,
}
```

TODO: Guests.

### Protocol

#### Case 1: Already logged in

```
L -------------------------------- G
LoginResult { success: true }
State change to QuickJoin
```

#### Case 2: Not logged in, login ok

```
L -------------------------------- G
AskForCredentials
                         Credentials
LoginResult { success: true }
State change to QuickJoin
```

#### Case 2: Not logged in, login bad

```
L -------------------------------- G
AskForCredentials
                         Credentials
LoginResult { success: false }
AskForCredentials
                         Credentials
LoginResult { success: false }
AskForCredentials
                         Credentials
LoginResult { success: false }
AskForCredentials
                         Credentials
LoginResult { success: true }
State change to QuickJoin
```

