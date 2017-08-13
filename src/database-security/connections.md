Database Connections
====================

## Connection string protection

To keep your connection strings secure, it's always a good practice to put the
authentication details on a separated configuration file outside public access.

Instead of placing your configuration file at `/your/app/path/public_html/`,
consider using a restricted access location `/some/private/path/config.json`

```json
{
  "db": {
    "user": "f00",
    "pass": "f00?bar#ItsP0ssible",
    "host": "localhost",
    "port": "3306"
  }
}
```

Then you can load your `config.json` file on your JavaScript application:

```javascript
const fs = require('fs');

const configFilePath = '/some/private/path/config.json';
const configContent = fs.readFileSync(configFilePath).toString();
const config = JSON.parse(configContent);

console.log('Configuration loaded');
```

Of course, if the attacker has root access, he could see the file what brings
us to the most cautious thing you can do - encrypt the file.

Remind that instead of `require`ing the configuration file, it was loaded as a
regular text file and then parsed as JSON: this may prevent code execution in
case of configuration file gets compromised.

## Database Credentials

You should use different credentials for every trust distinction and level:

* User
* Read-only user
* Guest
* Admin

That way if a connection is being made for a read-only user, they could never
mess up with your database information because the user actually can only read
the data.


