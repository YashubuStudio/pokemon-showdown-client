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
Open `play.pokemonshowdown.com/testclient.html` in your browser and append the host you started in step 5. If your server is reachable at `192.168.0.193`, use the following URL:

```
http://192.168.0.193:8080/play.pokemonshowdown.com/testclient.html?~~http://192.168.0.193/:8058
```

You can adjust the host and port numbers to match your environment.

If you are opening the file directly from disk, some browsers may escape the `?`. Running a small HTTP server (for example `npx http-server` from this repository) can avoid this.

When you first open the page, the client may display a warning asking you to copy text from a box and paste it back in. This happens because the test client does not have permission to read your browser's cookies directly. Simply follow the instructions once to complete the login. Creating `config/testclient-key.js` (see below) will prevent this prompt from showing on subsequent visits.

## 8. Optional: test client login key

To skip the warning about copying text every time you open `testclient.html`, you can store your session ID in a small file.

1. **Login manually once.** When the browser asks you to paste text back in, do so and wait for the client to finish logging in.
2. **Open your browser’s developer tools** (usually `F12` or right click > *Inspect*). Navigate to the Cookies section for your local server (for example `http://192.168.0.193:8058`).
3. **Find the cookie named `sid`**. Copy its entire value.
4. In this repository, create a new file `config/testclient-key.js` containing:

   ```javascript
   const POKEMON_SHOWDOWN_TESTCLIENT_KEY = '<sid-value>'; // paste the sid value here
   ```

   Replace `<sid-value>` with the value you copied in step 3.
5. Refresh the test client. If the value is correct, it will log you in automatically. If the manual prompt appears again, your key was not correct or has expired; repeat the steps to get a new `sid`.

## Summary

1. Clone and build the server.
2. Set `config/config.json` to reference your local host (e.g. `192.168.0.193`).
3. Start the server with `node pokemon-showdown`.
4. Build this client and open `play.pokemonshowdown.com/testclient.html` pointing to your server (for instance the URL shown above).

This setup allows you to test changes and run simulations locally without sending data to the public Pokémon Showdown servers.

