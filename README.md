<h1 align="center">Allora Network Point Program</h1>

1. Connect to Allora [dashboard](https://app.allora.network?ref=eyJyZWZlcnJlcl9pZCI6ImZjNzFjMTI0LTE3OTEtNGYxYS1hOWY3LTgwZDdmZWEyNjBjMiJ9) to find your Allora address

>

3. Get uAllo faucet [here](https://faucet.testnet-1.testnet.allora.network/)

>

3. You can check Allora network explorer [here](https://explorer.testnet-1.testnet.allora.network/allora-testnet-1)

>

<h1 align="center">Price Prediction Worker Node</h1>

## Install dependecies

```console
sudo apt update & sudo apt upgrade -y

sudo apt install tilde ca-certificates zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev curl git wget make jq build-essential pkg-config lsb-release libssl-dev libreadline-dev libffi-dev gcc screen unzip lz4 -y
```

## Install Python3

```console
sudo apt install python3
python3 --version

sudo apt install python3-pip
pip3 --version
```

## Install Docker

_note_

**if you had other node running using docker then SKIP THIS STEP!**
**and continue to next step**

```console
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
docker version

VER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)

curl -L "https://github.com/docker/compose/releases/download/"$VER"/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

# Install Go

```console
sudo rm -rf /usr/local/go
curl -L https://go.dev/dl/go1.22.4.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> $HOME/.bash_profile
source .bash_profile
go version
```

## Install Allorad: Wallet

```console
git clone https://github.com/allora-network/allora-chain.git

cd allora-chain && make all

allorad version-
```

## Add Wallet

- You can use your keplr seed-phrase to recover your wallet or create a new one

```console
# Recover your wallet with seed-phrase
allorad keys add testkey --recover

#OR

# Create a new wallet
allorad keys add testkey
```

## Install Worker

```console
# Install
cd $HOME && git clone https://github.com/allora-network/basic-coin-prediction-node

cd basic-coin-prediction-node
```

## Config Worker

```console
# Remove config file
rm -rf config.json

# Create new config file
tilde config.json
```

**Paste below code in it**

- Replace your wallet `Seed Phrase`
- `addressKeyName` was set as testkey since we choose it in step: Add Wallet

```
{
    "wallet": {
        "addressKeyName": "testkey",
        "addressRestoreMnemonic": "Seed Phrase",
        "alloraHomeDir": "",
        "gas": "1000000",
        "gasAdjustment": 1.0,
        "nodeRpc": "https://sentries-rpc.testnet-1.testnet.allora.network/",
        "maxRetries": 1,
        "delay": 1,
        "submitTx": false
    },
    "worker": [
        {
            "topicId": 1,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 2,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 7,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        }
    ]
}
```

Ctrl+X+Y+Enter to save & exit

## Run Worker

```console
chmod +x init.config
./init.config
```

- If you need to make changes to your `config.json` , you must rerun this command again after your changes are done

```console
docker compose up -d --build
```

## Check logs

Containers:

```console
docker compose ps
```

worker:

```console
docker compose logs -f worker
```

inference:

```console
docker compose logs -f inference
```

updater:

```console
docker compose logs -f updater

# Response:
# updater-basic-eth-pred  | UPDATING INFERENCE WORKER DATA
# updater-basic-eth-pred  | Response content is '0'
```

# You are going to receive points in the [dashboard](https://app.allora.network/points/leaderboard) in the next few hours
