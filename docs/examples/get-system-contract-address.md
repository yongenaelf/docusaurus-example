---
sidebar_position: 3
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Get a system contract address

Get a system contract address, take `AElf.ContractNames.Token` as an example

<Tabs groupId="programming-languages">
  <TabItem value="javascript" label="Javascript">

```js
const newWallet = AElf.wallet.createNewWallet();
const tokenContractName = "AElf.ContractNames.Token";
let tokenContractAddress;
(async () => {
  // get chain status
  const chainStatus = await aelf.chain.getChainStatus();
  // get genesis contract address
  const GenesisContractAddress = chainStatus.GenesisContractAddress;
  // get genesis contract instance
  const zeroContract = await aelf.chain.contractAt(
    GenesisContractAddress,
    newWallet
  );
  // Get contract address by the read only method `GetContractAddressByName` of genesis contract
  tokenContractAddress = await zeroContract.GetContractAddressByName.call(
    AElf.utils.sha256(tokenContractName)
  );
})();
```

  </TabItem>
  <TabItem value="dotnet" label="C#">

```csharp
// Get token contract address.
var tokenContractAddress = await client.GetContractAddressByNameAsync(HashHelper.ComputeFrom("AElf.ContractNames.Token"));
```

  </TabItem>
  <TabItem value="go" label="Go">

```go
// Get token contract address.
tokenContractAddress, _ := aelf.GetContractAddressByName("AElf.ContractNames.Token")
```

  </TabItem>
  <TabItem value="java" label="Java">

```java
// Get token contract address.
String tokenContractAddress = client.getContractAddressByName(privateKey, Sha256.getBytesSha256("AElf.ContractNames.Token"));
```

  </TabItem>
  <TabItem value="php" label="PHP">

```php
require_once 'vendor/autoload.php';
use AElf\AElf;
$url = '127.0.0.1:8000';
$aelf = new AElf($url);

$privateKey = 'XXX';
$bytes = new Hash();
$bytes->setValue(hex2bin(hash('sha256', 'AElf.ContractNames.Token')));
$contractAddress = $aelf->GetContractAddressByName($privateKey, $bytes);
```

  </TabItem>
  <TabItem value="python" label="Python">

```python
from aelf import AElf

aelf = AElf('http://127.0.0.1:8000')
// get genesis contract address
genesis_contract_address = aelf.get_genesis_contract_address_string()

// get contract address
// in fact, get_system_contract_address call the method 'GetContractAddressByName' in the genesis contract to get other contracts' address
multi_token_contract_address = aelf.get_system_contract_address('AElf.ContractNames.Token')
```

  </TabItem>
</Tabs>
