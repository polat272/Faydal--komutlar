<h1 align="center">Faydalı Komutlar </h1>

### Merhabalar, Node kurulumundan sonra sık sık ihtiyacımız olan komutları bir araya getirmeye çalıştım..

<img src="https://media.giphy.com/media/pOKrXLf9N5g76/giphy.gif" align="center" width="400" height="300"></a>

# Log kontrol
```
journalctl -fu nodenamed -o cat
journalctl -u nodenamed -f
```
# Sistemi başlat
```
sudo systemctl start nodenamed
```

# Sistemi durdur
```
sudo systemctl stop nodenamed
```
# Sistemi yeniden başlat
```
sudo systemctl restart nodenamed
```
# Dosyaya giriş
```
nano /root/.nodenamed/config/config.toml
```
# Dosyaya girip pruning kapatma
```
nano /root/.nodenamed/config/app.toml
```

### Düğüm Bilgisi

# Senkorizasyon bilgisi
```
nodenamed status 2>&1 | jq .SyncInfo
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
