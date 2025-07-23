# Setting up a Local Pokémon Showdown Environment

This guide outlines the steps required to run your own Pokémon Showdown server alongside this client in order to avoid depending on the public servers.

## 1. Install prerequisites

1. Install **Node.js v20** or later from [nodejs.org](https://nodejs.org/).
2. Ensure **git** is installed so that you can clone repositories.

## 2. Clone the server repository

The official server code is hosted at [https://github.com/Zarel/Pokemon-Showdown](https://github.com/Zarel/Pokemon-Showdown). Clone it somewhere on your machine:

```bash
git clone https://github.com/Zarel/Pokemon-Showdown.git
cd Pokemon-Showdown
```

## 3. Install server dependencies and build

From inside the `Pokemon-Showdown` directory, install the dependencies and build the server:

```bash
npm install --production
node build
```

The build step compiles necessary resources for the server to run.

## 4. Configure the server for local use

The server configuration is stored in `config/config-example.json`. Copy it to `config/config.json` if it does not exist and edit the following fields to point everything to your local machine:

```json
{
  "loginserver": "http://localhost:8058/",
  "routes": {
    "root": "localhost:8058",
    "client": "localhost:8058",
    "dex": "localhost:8058",
    "replays": "localhost:8058"
  }
}
```

Leave other settings as their defaults or adjust as needed. The important part is that the login server and route URLs use your local machine.

## 5. Start the server

Run the server with:

```bash
node pokemon-showdown
```

By default it listens on port `8058`. If you changed the port in your configuration, use that port.

## 6. Build this client

From this repository (`pokemon-showdown-client`), build the client so that `testclient.html` can connect to your local server:

```bash
cd ../pokemon-showdown-client
npm install
./build
```

## 7. Launch the test client

The test client is located in the `play.pokemonshowdown.com` folder after building.
Open `play.pokemonshowdown.com/testclient.html` in your browser and append the host you started in step 5. For example:

```
http://localhost:8080/play.pokemonshowdown.com/testclient.html?~~localhost:8058
```

If you are opening the file directly from disk, some browsers may escape the `?`. Running a small HTTP server (for example `npx http-server` from this repository) can avoid this.

## 8. Optional: test client login key

For easier login while developing, create `config/testclient-key.js` in the client repository with:

```javascript
const POKEMON_SHOWDOWN_TESTCLIENT_KEY = 'sid';
```

Replace `'sid'` with the value of the `sid` cookie from your server after logging in once. This lets you refresh without re-entering login data.

## Summary

1. Clone and build the server.
2. Set `config/config.json` to reference `localhost`.
3. Start the server with `node pokemon-showdown`.
4. Build this client and open `play.pokemonshowdown.com/testclient.html` pointing to the local server.

This setup allows you to test changes and run simulations locally without sending data to the public Pokémon Showdown servers.

