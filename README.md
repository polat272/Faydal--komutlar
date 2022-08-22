<h1 align="center">Faydalı Komutlar </h1>

### Merhabalar, Node kurulumundan sonra sık sık ihtiyacımız olan komutları bir araya getirmeye çalıştım..

![image]("https://media.giphy.com/media/EoH4Wpu8suiNTLpI6j/giphy.gif")

# Log kontrol
```
journalctl -fu nodenamed -o cat
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
