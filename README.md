# maker-pricefeed-tools

This repo contains tools that are useful for running a Maker price feed using [`setzer`](https://github.com/makerdao/setzer). This repo follows the instructions found at [https://makerdao.com/feed-manual](https://makerdao.com/feed-manual).

## Instructions

### Configuration

Rename `defaults.env` to `.env` and begin to set the variables for each of the below (note [`.env`](https://docs.docker.com/compose/environment-variables/#the-env-file) is a specifically recognized filename by `docker-compose`).

Note that the `docker-compose.yml` file expects your keystore folder to be `./secrets/keys/ethereum` and your password file to be `./secrets/pass`.

| Variable          | Value         |
| ----------------- | ------------- |
| **ETH_FROM**          | The public key you'll be using to sign price feed updates
| **ETH_RPC_URL**       | The RPC URL of your Ethereum node. For example, Infura would be `https://mainnet.infura.io/<your api key>`. If using the built-in Parity container, `http://parity:8545`. |
| **ETH_KEYSTORE**      | /root/.local/share/io.parity.ethereum/keys/ethereum/ |
| **ETH_PASSWORD**      | /tmp/passwords/pass |
| **SETZER_FEED**       | The feed address you will get from following steps in the next section. |
| **SETZER_MEDIANIZER** | 0x729D19f657BD0614b4985Cf1D82531c67569197B | 
| **SETZER_SOURCES**    | Your choice of price feeds. See choices by running `docker-compose run setzer price ls` |

### Deploying your price feed

Next, use `seth` to deploy your price feed. It will respond with an address that represents your price feed address.

```bash
docker-compose run --entrypoint seth setzer send 0x435AD5CAE6eA3e065ce540464366b71Fba8f0c52 \
	--guess "create()(address)" \
	-F [YOUR ACCOUNT] \
	-G 700000 \
	-P 60000000000
```

Take the price feed address and use it to set `SETZER_FEED` in your environment file.

### Testing setzer bot

Once setzer is configured, it's time to test it. Run the following command:

```bash
docker-compose run setzer bot
```

It will initialize with your current configuration. The first time, your price-feed is empty so it will update with the new price.

Since you ran setzer bot, it will ask for confirmation before submitting a transaction. When prompted, select Y or YES to send the transaction. Wait for your transaction to be mined. If it's not mined in 90 seconds, setzer will try to resend with higher gas price. Accept the transaction until it passes.

