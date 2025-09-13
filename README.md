## Getting Started

- First of all you need a full bitcoin node, this compose file doesn't include
  one. You can use [Bitcoin Knots](https://bitcoinknots.org/).
- Once you have a Bitcoin node, make sure to include the following in your in
  `bitcoin.conf`:
  ```
  # Enable RPC server
  server=1
  rpcbind=0.0.0.0
  rpcallowip=0.0.0.0/0
  rpcuser=knots
  rpcpassword=password123

  # Notify Datum of new blocks
  blocknotify=wget -q -O /dev/null http://localhost:7152/NOTIFY

  # Needed for Electrum server and Mempool Space
  txindex=1
  blockfilterindex=1
  zmqpubrawblock=tcp://0.0.0.0:28332
  zmqpubrawtx=tcp://0.0.0.0:28333
  ```
- Make sure to change `rpcuser` and `rpcpassword` to something more secure than
  the example above.
- Once you change your `bitcoin.conf`, restart your Bitcoin node and wait for it
  to fully sync.
- Next open the `config.env` file and change the variables to match your Bitcoin
  node configuration. Make sure the `BITCOIN_DATA` path matches your Bitcoin
  node data directory (LLMs might help you with that based on your OS). And
  don't forget to set Datum variables.
- Make sure you have docker installed on your machine.
- Now you can start the services using:
  ```
  docker compose --env-file=config.env up -d
  ```
  Make sure you run this command in the same directory where the
  `docker-compose.yaml` file is located.
- Some of these services might take a while to fully index the blockchain before
  they are fully functional such Electrum server and Mempool Space, so be
  patient. Also while building the image for the first time, compiling electrs
  might take a while depending on your machine, because of Rust.
- You can watch the compose logs using:
  ```
  docker compose --env-file=config.env logs -f
  ```
- To stop the services you can use:
  ```
  docker compose --env-file=config.env down
  ```

## Services

- Datum: `http://localhost:7152` (Admin Interface)
- Datum Stratum: `stratum+tcp://localhost:23334` (Mining)
- Core Lightning: `http://localhost:2103` (Web App)
- Mempool Space: `http://localhost:8998` (Web App)
- Electrum Server: `localhost:50001` (Electrum Wallets)
