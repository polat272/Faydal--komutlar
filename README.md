<h1 align="center">:dizzy:Faydalı Komutlar:dizzy: </h1>

### :dizzy: Merhabalar, Node kurulumundan sonra sık sık ihtiyacımız olan komutları bir araya getirmeye çalıştım.. :dizzy:

:point_right: <a href="https://discord.gg/discord.gg/ruescommunity" target="blank"><img :point_right:align="right" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/discord.svg" :point_left: alt="discord.gg/ruescommunity" height="40" width="40" /></a> :point_left:

<img src="https://media.giphy.com/media/pOKrXLf9N5g76/giphy.gif" align="center" width="400" height="300"></a>

# Log kontrol
```
journalctl -fu nodenamed -o cat
journalctl -u nodenamed -f
```
# Sistem hafıza ve ram cpu kontrolü
```
df -h
df 
htop
vmstat top

```
# Sistemi başlat
```
sudo systemctl start nodenamed

systemctl restart systemd-journald

nodenamed start
```

# Sistemi durdur
```
sudo systemctl stop nodenamed
```
# Sistemi yeniden başlat
```
sudo systemctl restart nodenamed
```
# Peer ekleme komutu ve ögrenme
```
PEERS=""
sed -i "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/;" $HOME/.nodename/config/config.toml
systemctl restart nodenamed
echo "$(nodenamed tendermint show-node-id)@$(curl ifconfig.me):26656"
```
# Dosyaya giriş
```
nano /root/.nodenamed/config/config.toml
```
# Dosyaya girip pruning kapatma
```
nano /root/.nodenamed/config/app.toml
```
# Gprc ve Rpc adresi öğrenme
```
echo "$(curl -s ifconfig.me)$(grep -A 6 "\[grpc\]" ~/.nodename/config/app.toml | egrep -o ":[0-9]+")"

echo "$(curl -s ifconfig.me)$(grep -A 3 "\[rpc\]" ~/.nodename/config/config.toml | egrep -o ":[0-9]+")"
```
### Düğüm Bilgisi

# Senkorizasyon bilgisi
```
nodenamed status 2>&1 | jq .SyncInfo
```
# Senkorizasyon tahmin süresi
```
```
# Validator bilgisi
```
nodenamed status 2>&1 | jq .ValidatorInfo
```
# Node bilgisi
```
nodenamed status 2>&1 | jq .NodeInfo
```
# Node ıd bilgisi
```
nodenamed tendermint show-node-id
```

### Cüzdan işlemleri

# Cüzdan listesi
```
nodenamed keys list
```
# Cüzdan kurtarma 
```
nodenamed keys add $walletname --recover
```
# Cüzdan bakiyesi öğrenme
```
nodenamed query bank balances $walletadres
```
# Token transferi
```
nodenamed tx bank send $walletadresgönderici <$walletadresalıcı> 100000uatom
```
# Oylama için kullanılan komut
```
nodenamed tx gov vote 1 yes --from $walletname --chain-id=$$CHAİN_ID
```

### Stake, Delege ve Ödüller
# Delege stake
```

nodenamed tx staking delegate $valoperadress 10000000uatom --from=$walletname --chain-id=$CHAIN_ID --gas=auto
```
# Redelege stake'den validatorden başka bir validatöre gönderme
```
nodenamed tx staking redelegate <göndericivaloperadres> <$alıcıvaloperadres> 10000000uatom --from=$Wwalletname --chain-id=$CHAIN_ID --gas=auto
```
# Tüm ödülleri çek
```
nodenamed tx distribution withdraw-all-rewards --from=$walletname --chain-id=$CHAIN_ID --gas=auto
```
# Komisyon ile ödülleri çek
```
nodenamed tx distribution withdraw-rewards $valoperadress --from=$walletname --commission --chain-id=$CHAIN_ID
```

# IBC staking-reedem-claim komutları
```
nodenamed tx stakeibc liquid-stake 1000 uatom --from $CÜZDAN ADINIZ --chain-id $CHAIN_ID --gas auto -y

nodenamed tx stakeibc redeem-stake 500 $CHAIN $chainadres  --chain-id $CHAIN_ID --from $CÜZDANADIN --gas 500000 -y

nodenamed tx stakeibc claim-undelegated-tokens $CHAIN8claimedecegimizag $epocnumber $çekilecekadres --chain-id $CHAIN_ID --from $cuzdanadı -y

```
# IBC transferinde stake ettiğiğmiz tokenleri claim ederken epocnumber değişik olabilir bulmak için komut
```
nodenamed q records list-user-redemption-record --limit 10000 --output json | jq  '.UserRedemptionRecord | map(select(.sender == "CÜZDANADRESİN"))'
```

### Validator komutları

# Validator düzenle
```
nodenamed tx staking edit-validator \
  --moniker=$node_takma_ad \
  --identity="" \
  --website="<web_sitenin_adresi>" \
  --details="<detay>" \
  --chain-id=$CHAIN_ID \
  --from=$walletname
  ```
# Unjail Komutu
  ```
  nodenamed tx slashing unjail \
    --broadcast-mode=block \
    --from=$walletname
    --chain-id=$CHAIN_ID
    --gas=auto
```
### Düğümü Sil
 ```
sudo systemctl stop nodenamed
sudo systemctl disable nodenamed
sudo rm /etc/systemd/system/nodename* -rf
sudo rm $(which nodenamed) -rf
sudo rm $HOME/.nodename* -rf
sudo rm $HOME/nodename -rf
sed -i '/CHAIN_ID_/d' ~/.bash_profile
```
# Terminalden Validator Kontrolü(explorerde görünmediği zaman terminalden kontrol edilebilir ENTER tuşu ile isimler akar)
```
nodenamed query staking validators --limit 2000 -o json | jq -r '.validators[] | select(.status=="BOND_STATUS_UNBONDED") | [.operator_address, .status, (.tokens|tonumber / pow(10; 6)), .description.moniker] | @csv' | column -t -s"," | sort -k3 -n -r| less
```
