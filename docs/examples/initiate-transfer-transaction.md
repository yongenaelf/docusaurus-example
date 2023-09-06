---
sidebar_position: 4
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Initiate a transfer transaction

The following example demonstrates how a transfer transaction may be initiated.

<Tabs groupId="programming-languages">
  <TabItem value="javascript" label="Javascript">

```js
const wallet = AElf.wallet.createNewWallet();
let tokenContract;
// Use token contract for examples to demonstrate how to get a contract instance in different ways
// in async function
(async () => {
  tokenContract = await aelf.chain.contractAt(tokenContractAddress, wallet);
})();

// promise way
aelf.chain.contractAt(tokenContractAddress, wallet).then((result) => {
  tokenContract = result;
});

// callback way
aelf.chain.contractAt(tokenContractAddress, wallet, (error, result) => {
  if (error) throw error;
  tokenContract = result;
});

(async () => {
  // get the balance of an address, this would not send a transaction,
  // or store any data on the chain, or required any transaction fee, only get the balance
  // with `.call` method, `aelf-sdk` will only call read-only method
  const result = await tokenContract.GetBalance.call({
    symbol: "ELF",
    owner: "XXX",
  });
  console.log(result);
  /**
      {
        "symbol": "ELF",
        "owner": "XXX",
        "balance": "1000000000000"
      }*/
  // with no `.call`, `aelf-sdk` will sign and send a transaction to the chain, and return a transaction id.
  // make sure you have enough transaction fee `ELF` in your wallet
  const transactionId = await tokenContract.Transfer({
    symbol: "ELF",
    to: "XXX",
    amount: "1000000000",
    memo: "transfer in demo",
  });
  console.log(transactionId);
  /**
        {
          "TransactionId": "123123"
        }
      */
})();
```

  </TabItem>
  <TabItem value="dotnet" label="C#">

```csharp
// Get token contract address.
var tokenContractAddress = await client.GetContractAddressByNameAsync(HashHelper.ComputeFrom("AElf.ContractNames.Token"));

var methodName = "Transfer";
var param = new TransferInput
{
    To = new Address {Value = Address.FromBase58("XXX").Value},
    Symbol = "ELF",
    Amount = 1000000000,
    Memo = "transfer in demo"
};
var ownerAddress = client.GetAddressFromPrivateKey(PrivateKey);

// Generate a transfer transaction.
var transaction = await client.GenerateTransaction(ownerAddress, tokenContractAddress.ToBase58(), methodName, param);
var txWithSign = client.SignTransaction(PrivateKey, transaction);

// Send the transfer transaction to AElf chain node.
var result = await client.SendTransactionAsync(new SendTransactionInput
{
    RawTransaction = txWithSign.ToByteArray().ToHex()
});

await Task.Delay(4000);
// After the transaction is mined, query the execution results.
var transactionResult = await client.GetTransactionResultAsync(result.TransactionId);
Console.WriteLine(transactionResult.Status);

// Query account balance.
var paramGetBalance = new GetBalanceInput
{
    Symbol = "ELF",
    Owner = new Address {Value = Address.FromBase58(ownerAddress).Value}
};
var transactionGetBalance =await client.GenerateTransaction(ownerAddress, tokenContractAddress.ToBase58(), "GetBalance", paramGetBalance);
var txWithSignGetBalance = client.SignTransaction(PrivateKey, transactionGetBalance);

var transactionGetBalanceResult = await client.ExecuteTransactionAsync(new ExecuteTransactionDto
{
    RawTransaction = txWithSignGetBalance.ToByteArray().ToHex()
});

var balance = GetBalanceOutput.Parser.ParseFrom(ByteArrayHelper.HexstringToByteArray(transactionGetBalanceResult));
Console.WriteLine(balance.Balance);
```

  </TabItem>
  <TabItem value="go" label="Go">

```go
// Get token contract address.
tokenContractAddress, _ := aelf.GetContractAddressByName("AElf.ContractNames.Token")
fromAddress := aelf.GetAddressFromPrivateKey(aelf.PrivateKey)
methodName := "Transfer"
toAddress, _ := util.Base58StringToAddress("XXX")

params := &pb.TransferInput{
	To:     toAddress,
	Symbol: "ELF",
	Amount: 1000000000,
	Memo:   "transfer in demo",
}
paramsByte, _ := proto.Marshal(params)

// Generate a transfer transaction.
transaction, _ := aelf.CreateTransaction(fromAddress, tokenContractAddress, methodName, paramsByte)
signature, _ := aelf.SignTransaction(aelf.PrivateKey, transaction)
transaction.Signature = signature

// Send the transfer transaction to AElf chain node.
transactionByets, _ := proto.Marshal(transaction)
sendResult, _ := aelf.SendTransaction(hex.EncodeToString(transactionByets))

time.Sleep(time.Duration(4) * time.Second)
transactionResult, _ := aelf.GetTransactionResult(sendResult.TransactionID)
fmt.Println(transactionResult)

// Query account balance.
ownerAddress, _ := util.Base58StringToAddress(fromAddress)
getBalanceInput := &pb.GetBalanceInput{
	Symbol: "ELF",
	Owner:  ownerAddress,
}
getBalanceInputByte, _ := proto.Marshal(getBalanceInput)

getBalanceTransaction, _ := aelf.CreateTransaction(fromAddress, tokenContractAddress, "GetBalance", getBalanceInputByte)
getBalanceTransaction.Params = getBalanceInputByte
getBalanceSignature, _ := aelf.SignTransaction(aelf.PrivateKey, getBalanceTransaction)
getBalanceTransaction.Signature = getBalanceSignature

getBalanceTransactionByets, _ := proto.Marshal(getBalanceTransaction)
getBalanceResult, _ := aelf.ExecuteTransaction(hex.EncodeToString(getBalanceTransactionByets))
balance := &pb.GetBalanceOutput{}
getBalanceResultBytes, _ := hex.DecodeString(getBalanceResult)
proto.Unmarshal(getBalanceResultBytes, balance)
fmt.Println(balance)
```

  </TabItem>
  <TabItem value="java" label="Java">

```java
// Get token contract address.
String tokenContractAddress = client.getContractAddressByName(privateKey, Sha256.getBytesSha256("AElf.ContractNames.Token"));

Client.Address.Builder to = Client.Address.newBuilder();
to.setValue(ByteString.copyFrom(Base58.decodeChecked("XXX")));
Client.Address toObj = to.build();

TokenContract.TransferInput.Builder paramTransfer = TokenContract.TransferInput.newBuilder();
paramTransfer.setTo(toObj);
paramTransfer.setSymbol("ELF");
paramTransfer.setAmount(1000000000);
paramTransfer.setMemo("transfer in demo");
TokenContract.TransferInput paramTransferObj = paramTransfer.build();

String ownerAddress = client.getAddressFromPrivateKey(privateKey);

Transaction.Builder transactionTransfer = client.generateTransaction(ownerAddress, tokenContractAddress, "Transfer", paramTransferObj.toByteArray());
Transaction transactionTransferObj = transactionTransfer.build();
transactionTransfer.setSignature(ByteString.copyFrom(ByteArrayHelper.hexToByteArray(client.signTransaction(privateKey, transactionTransferObj))));
transactionTransferObj = transactionTransfer.build();

// Send the transfer transaction to AElf chain node.
SendTransactionInput sendTransactionInputObj = new SendTransactionInput();
sendTransactionInputObj.setRawTransaction(Hex.toHexString(transactionTransferObj.toByteArray()));
SendTransactionOutput sendResult = client.sendTransaction(sendTransactionInputObj);

Thread.sleep(4000);
// After the transaction is mined, query the execution results.
TransactionResultDto transactionResult = client.getTransactionResult(sendResult.getTransactionId());
System.out.println(transactionResult.getStatus());

// Query account balance.
Client.Address.Builder owner = Client.Address.newBuilder();
owner.setValue(ByteString.copyFrom(Base58.decodeChecked(ownerAddress)));
Client.Address ownerObj = owner.build();

TokenContract.GetBalanceInput.Builder paramGetBalance = TokenContract.GetBalanceInput.newBuilder();
paramGetBalance.setSymbol("ELF");
paramGetBalance.setOwner(ownerObj);
TokenContract.GetBalanceInput paramGetBalanceObj = paramGetBalance.build();

Transaction.Builder transactionGetBalance = client.generateTransaction(ownerAddress, tokenContractAddress, "GetBalance", paramGetBalanceObj.toByteArray());
Transaction transactionGetBalanceObj = transactionGetBalance.build();
String signature = client.signTransaction(privateKey, transactionGetBalanceObj);
transactionGetBalance.setSignature(ByteString.copyFrom(ByteArrayHelper.hexToByteArray(signature)));
transactionGetBalanceObj = transactionGetBalance.build();

ExecuteTransactionDto executeTransactionDto = new ExecuteTransactionDto();
executeTransactionDto.setRawTransaction(Hex.toHexString(transactionGetBalanceObj.toByteArray()));
String transactionGetBalanceResult = client.executeTransaction(executeTransactionDto);

TokenContract.GetBalanceOutput balance = TokenContract.GetBalanceOutput.getDefaultInstance().parseFrom(ByteArrayHelper.hexToByteArray(transactionGetBalanceResult));
System.out.println(balance.getBalance());
```

  </TabItem>
  <TabItem value="php" label="PHP">

```php
require_once 'vendor/autoload.php';
use AElf\AElf;
$url = '127.0.0.1:8000';
// create a new instance of AElf
$aelf = new AElf($url);

// private key
$privateKey = 'XXX';

$aelfEcdsa = new BitcoinECDSA();
$aelfEcdsa->setPrivateKey($privateKey);
$publicKey = $aelfEcdsa->getUncompressedPubKey();
$address = $aelfEcdsa->hash256(hex2bin($publicKey));
$address = $address . substr($aelfEcdsa->hash256(hex2bin($address)), 0, 8);
// sender address
$base58Address = $aelfEcdsa->base58_encode($address);

// transaction input
$params = new Hash();
$params->setValue(hex2bin(hash('sha256', 'AElf.ContractNames.Vote')));

// transaction method name
$methodName = "GetContractAddressByName";

// transaction contract address
$toAddress = $aelf->getGenesisContractAddress();

// generate a transaction
$transactionObj = aelf->generateTransaction($base58Address, $toAddress, $methodName, $params);

//signature
$signature = $aelf->signTransaction($privateKey, $transactionObj);
$transactionObj->setSignature(hex2bin($signature));

// obj Dto
$executeTransactionDtoObj = ['RawTransaction' => bin2hex($transaction->serializeToString())];

$result = $aelf->sendTransaction($executeTransactionDtoObj);
print_r($result);
```

  </TabItem>
  <TabItem value="python" label="Python">

```python
from aelf import AElf

url = 'http://127.0.0.1:8000'
// create a new instance of AElf
aelf = AElf(url)

// generate the private key
private_key_string = 'XXX'
private_key = PrivateKey(bytes(bytearray.fromhex(private_key_string)))

// create input, the type is generated by protoc
cross_chain_transfer_input = CrossChainTransferInput()

// generate the transaction
transaction = aelf.create_transaction(to_address, method_name, params.SerializeToString())

// sign the transaction by user's private key
aelf.sign_transaction(private_key, transaction)

// execute the transaction
aelf.execute_transaction(transaction)
```

  </TabItem>
</Tabs>
