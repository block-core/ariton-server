# ariton-server
Repo for a future managed server that hosts DWNs and other features for Ariton users


## nginx

The DWN Server software will add certain HTTP headers to the response, including the Allow-Origin. Hence this should not be
added by nginx.

Example nxing site config:

```nginx
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Add CORS headers
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'dwn-request, Origin, X-Requested-With, Content-Type, Accept, Authorization';

        # Handle preflight requests
        if ($request_method = 'OPTIONS') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'dwn-request, Origin, X-Requested-With, Content-Type, Accept, Authorization';
            add_header 'Access-Control-Max-Age' 1728000;
            add_header 'Content-Type' 'text/plain charset=UTF-8';
            add_header 'Content-Length' 0;
            return 204;
        }
    }
```

## Running from source

The best option is to run from source:

```sh
git clone REPO

npm install

npm run server
```

## Hosting DWN

Running a DWN Server is a simple process. You can run the server on your local machine or on a remote server.

```sh
npm init

npm install @web5/dwn-server
```

Create a file called `server.js` and add the following code:

```js
import { DwnServer } from '@web5/dwn-server';

const server = new DwnServer();

server.start();
```

If you get an error during npm install, like an error installing `better-sqlite3`, you can fix this by:

# Install build dependencies
```sh
sudo apt-get update
sudo apt-get install -y python3 build-essential gcc make
```
# Clear npm cache
```sh
npm cache clean --force
```

# Retry installation
```sh
npm install better-sqlite3 --build-from-source
```

Then run again:

```sh
npm install @web5/dwn-server
```

If you're still getting errors, try installing additional dependencies:
```sh
sudo apt-get install -y python3-pip python3-dev
```

The issue might also be related to the Node version, then perform the following steps:

# Switch to Node.js LTS (18.x)
```sh
nvm install 18
nvm use 18
```

# Install build dependencies
```sh
sudo apt-get update
sudo apt-get install -y python3 build-essential gcc g++ make
```

# Clear previous build artifacts and cache
```sh
rm -rf node_modules
rm package-lock.json
npm cache clean --force
```

# Reinstall packages
```sh
npm install @web/dwn-server
```