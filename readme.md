
# Cách để chạy contract trên Oraichain

## Install Oraicli 
Để tương tác với oraichain thì bạn cần cài đặt CLI:
```
git clone https://github.com/oraichain/oraicli.git
```
Install dependencies:
```
yarn 
```

Sau đó hãy tạo file .env và copy mnemonic của wallet bạn vào đây: 
```
SEND_MNEMONIC=COPY_HERE
URL=http://testnet-lcd.orai.io
DENOM=orai
CHAIN_ID=Oraichain-testnet
RPC_URL=https://testnet-rpc.orai.io
HD_PATH=m/44'/118'/0'/0/0
DENOM=orai
PREFIX=orai
```

Ví dụ wallet của mình là: 
```
SEND_MNEMONIC=horn grant bla bla bla bla 
URL=http://testnet-lcd.orai.io
DENOM=orai
CHAIN_ID=Oraichain-testnet
RPC_URL=https://testnet-rpc.orai.io
HD_PATH=m/44'/118'/0'/0/0
DENOM=orai
PREFIX=orai
```

Sau khi hoàn thành xong hãy chạy thử lệnh nhé 
```
yarn oraicli wasm
```

![image](images/Pasted%20image%2020231123163756.png)



## Deloy smart contract 
Tạo một terminal mới
tải source có sẵn này đi nhé: 
```
git clone https://github.com/hien-p/Oraichain101.git
```
Sau đó chạy lệnh docker để deloy ra file .wasm:
```
docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/optimizer:0.15.0
```

Tham khảo từ đây nhé: 
```
https://github.com/CosmWasm/rust-optimizer
```
Kết quả mà chạy ra được như dưới đây là Ok: 

![image](images/Pasted%20image%2020231123165439.png)



Sau đó cd vào folder artifacts/ sẽ thấy tên file.wasm. Trong trường hợp này thì tên là cosmwasm_poc.wasm. 

Quay lại folder oraicli mà đã chạy ở trên để chạy lệnh này: 
```
yarn oraicli wasm upload <path_to_wasm_contract_file> --fees 2500orai
```

Trong đó Path to wasm contract file là path của  cosmwasm_poc.wasm nhé. 
Ví dụ trong trường hợp này của mình sẽ là:
```
media/hienpy/data/dev/oraichain/dapps/cosmwasm_poc/cosmwasm-
poc/artifacts/cosmwasm_poc.wasm
```
![image](images/Pasted%20image%2020231123165743.png)

Sau khi chạy bạn sẽ thấy một code -id. Của mình là 6471 
bạn có thể chạy lệnh này của mình dựa trên code -id hoặc lấy code-id mà bạn vừa có đều được 
```
yarn oraicli wasm instantiate --code-id 6471 --input '{"count": 11}' --label
"voting"
```
![image](images/Pasted%20image%2020231123165922.png)


Kết quả là ta sẽ được một address giống như này:
```
orai17mq0sg6ey9u2c5pwjjn8l6sppacrrzfucx5r97pcn25d6mjuax2qza6jar
```

Đây chính là contract ID của bạn. Ta có thể tương tác query/ execute được rồi đó. 
Thử query nhé: 
Args là contract id của bạn nhé 
```
yarn oraicli wasm query
orai17mq0sg6ey9u2c5pwjjn8l6sppacrrzfucx5r97pcn25d6mjuax2qza6jar --input
'{"get_count": {}}'
```
![image](images/Pasted%20image%2020231123172117.png)

Ngược lại có thể chạy lệnh này để execute:
```
yarn oraicli wasm execute
orai17mq0sg6ey9u2c5pwjjn8l6sppacrrzfucx5r97pcn25d6mjuax2qza6jar --input
'{"increment": {}}'
```

Và kết quả: 
![image](images/Pasted%20image%2020231123172310.png)

Tuy nhiên nếu bạn lười gõ lệnh CLI giống như mình thì hãy chạy Cosmwasm IDE và truyền args y hệt như trên. 
Nguồn để cài đặt đây nhé:
```
https://docs.orai.io/developers/cosmwasm-ide
```

Mình lấy ví dụ cho việc chạy Query: 

![image](images/Pasted%20image%2020231123172608.png)


Trong bài viết tiếp theo  mình đang nghiên cứu tiếp về:
- [ ] Cách dùng AI service của oracles lên on chain sẽ như thế nào thông qua CLI tools
* [ ] Cách tạo một Fungible Token (cw20 ) trên oraichain
* [ ] Swap token đó với native token(đang chưa biết như thế nào mấy ní ơi :V )
Ngoài ra bạn không nhất thiết phải dùng Oraicli. Oraichain cũng vừa build một tools khá xịn là [Cosmwasm-tools](https://github.com/oraichain/cosmwasm-tools) . Cho phép bạn build contract và tương tác luôn mà không cần lòng vòng giống mình. Hi vọng mình sẽ có nhiều thời gian hơn để viết về cái này. 

