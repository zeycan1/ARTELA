![artela](https://github.com/molla202/Artela/assets/91562185/a7922117-442e-4bbf-b56a-1d11e09670f7)

## Linkler:
 * [Topluluk kanalımız](https://t.me/corenodechat)
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)
 * [Artela Resmi Websitesi](https://artela.network/)
 * [Artela Resmi Twitter](https://twitter.com/Artela_Network)
 * [Artela Resmi Discord](https://discord.gg/TzmnmuCU)
 * [Artela Dökümantasyon](https://docs.artela.network/develop/node/run-full-node)
 * [Artela Tüm Linkler](https://linktr.ee/artela_network)
 * [Artela EXPLORER](https://test.explorer.ist/artela/staking)




## Sistem Gereksinimleri

| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| ✔️CPU |	4 vcpu|
| ✔️RAM	| 8 GB |
| ✔️Storage	| 200+ GB SSD |
| ✔️UBUNTU | 20or22 |


### Update ve gereklilikler
```
apt update && apt upgrade -y
apt install curl iptables build-essential git wget jq make gcc nano tmux htop lz4 nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```
### Go kurulum
```
ver="1.20.3"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```
### Varyasyonlar (moniker ve cüzdan adınızı değiştiriniz)
```
echo "export WALLET="wallet"" >> $HOME/.bash_profile
echo "export MONIKER="test"" >> $HOME/.bash_profile
echo "export ARTELA_CHAIN_ID="artela_11822-1"" >> $HOME/.bash_profile
echo "export ARTELA_PORT="45"" >> $HOME/.bash_profile
source $HOME/.bash_profile

cd $HOME
rm -rf artela
git clone https://github.com/artela-network/artela
cd artela
git checkout v0.4.7-rc4
make install

artelad config chain-id artela_11822-1
artelad init "$MONIKER" --chain-id artela_11822-1

wget -qO $HOME/.artelad/config/genesis.json https://raw.githubusercontent.com/Core-Node-Team/scripts/main/artela/genesis.json
wget -qO $HOME/.artelad/config/addrbook.json https://raw.githubusercontent.com/Core-Node-Team/scripts/main/artela/addrbook.json
SEEDS=""
PEERS="a996136dcb9f63c7ddef626c70ef488cc9e263b8@144.217.68.182:22256,de5612c035bd1875f0bd36d7cbf5d660b0d1e943@5.78.64.11:26656,bec6934fcddbac139bdecce19f81510cb5e02949@47.254.24.106:26656,30fb0055aced21472a01911353101bc4cd356bb3@47.89.230.117:26656,a03ae11a093c67e2554b73d174c4168fe715af10@57.128.103.184:26656,146d6011cce0423f564c9277c6a3390657c53730@157.90.226.23:26656,0188a9bcff4f411b29dbddda527d77803396e1c6@185.245.182.180:26656,b23bc610c374fd071c20ce4a2349bf91b8fbd7db@65.108.72.233:11656,aa416d3628dcce6e87d4b92d1867c8eca36a70a7@47.254.93.86:26656,978dee673bd447147f61aa5a1bdaabdfb8f8b853@47.88.57.107:26656,35ce36af33e289a29787eedb3127d21bf10edcff@81.0.218.194:45656,32d0e4aec8d8a8e33273337e1821f2fe2309539a@47.88.58.36:26656,1b73ac616d74375932fb6847ec67eee4a98174e9@116.202.85.52:25556,9e2fbfc4b32a1b013e53f3fc9b45638f4cddee36@47.254.66.177:26656,b23bc610c374fd071c20ce4a2349bf91b8fbd7db@65.108.72.233:11656,30fb0055aced21472a01911353101bc4cd356bb3@47.89.230.117:26656,9e2fbfc4b32a1b013e53f3fc9b45638f4cddee36@47.254.66.177:26656,978dee673bd447147f61aa5a1bdaabdfb8f8b853@47.88.57.107:26656,aa416d3628dcce6e87d4b92d1867c8eca36a70a7@47.254.93.86:26656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.artelad/config/config.toml

sed -i 's|^pruning *=.*|pruning = "custom"|g' $HOME/.artelad/config/app.toml
sed -i 's|^pruning-keep-recent  *=.*|pruning-keep-recent = "100"|g' $HOME/.artelad/config/app.toml
sed -i 's|^pruning-interval *=.*|pruning-interval = "10"|g' $HOME/.artelad/config/app.toml
sed -i 's|^snapshot-interval *=.*|snapshot-interval = 0|g' $HOME/.artelad/config/app.toml

sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.025art"|g' $HOME/.artelad/config/app.toml
sed -i 's|^prometheus *=.*|prometheus = true|' $HOME/.artelad/config/config.toml
sed -i 's|^indexer *=.*|indexer = "null"|' $HOME/.artelad/config/config.toml

# set custom ports in app.toml
sed -i.bak -e "s%:1317%:${ARTELA_PORT}317%g;
s%:8080%:${ARTELA_PORT}080%g;
s%:9090%:${ARTELA_PORT}090%g;
s%:9091%:${ARTELA_PORT}091%g;
s%:8545%:${ARTELA_PORT}545%g;
s%:8546%:${ARTELA_PORT}546%g;
s%:6065%:${ARTELA_PORT}065%g" $HOME/.artelad/config/app.toml

# set custom ports in config.toml file
sed -i.bak -e "s%:26658%:${ARTELA_PORT}658%g;
s%:26657%:${ARTELA_PORT}657%g;
s%:6060%:${ARTELA_PORT}060%g;
s%:26656%:${ARTELA_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${ARTELA_PORT}656\"%;
s%:26660%:${ARTELA_PORT}660%g" $HOME/.artelad/config/config.toml

sudo tee /etc/systemd/system/artelad.service > /dev/null << EOF
[Unit]
Description=artela node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which artelad) start
Restart=on-failure
RestartSec=10
LimitNOFILE=10000
[Install]
WantedBy=multi-user.target
EOF
```

### snap
```
artelad tendermint unsafe-reset-all --home $HOME/.artelad --keep-addr-book

curl -L http://37.120.189.81/artela_testnet/artela_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.artelad
```
### Başlatalım
```
sudo systemctl daemon-reload
sudo systemctl enable artelad
sudo systemctl start artelad
```
### Log kontrolü için
```
sudo journalctl -fu artelad -o cat
```

### Cüzdan olusturma

artelad keys add cüzdan-adı

### Not: faucet almak için cüzdan olusturduktan sonra altaki kodla adresinizi öğrenin. Address (EIP-55) kısmındaki 0X ile başalayan adresile faucetten tokenlerinizi isteyin. 6 saatte bir token isteyebilirsiniz. 
```
artelad debug addr art ile başlayan cüzdan adresiniz


### Validator olusturma
Not: Faucetten token aldıktan sonra validatör oluşturabilirsiniz. moniker ve cüzdan adınızı yazınız
```
artelad tx staking create-validator \
--amount 950000uart \
--from cüzdan-adınız \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(artelad tendermint show-validator) \
--moniker "moniker-adınız" \
--identity="" \
--website="" \
--chain-id artela_11822-1 \
--gas auto \
--gas-adjustment 1.4 \
--gas-prices 0.055uart \
--node http://localhost:45657 \
-y
```
### Delege
Not: cüzdanadı kısımlarını değiştirin.
```
artelad tx staking delegate $(artelad keys show cüzdanadı --bech val -a) 1.5art --from cüzdanadı --chain-id artela_11822-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uart --node http://localhost:45657 -y
```

## BAŞVURU FORMUNU DOLDURMAYI UNUTMAYIN: [TIKLA](https://t.co/EPAoUcWriG)https://t.co/EPAoUcWriG
