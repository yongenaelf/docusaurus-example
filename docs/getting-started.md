---
sidebar_position: 1
---

# Getting Started

## Introduction

This site provides some documentation for SDKs that you may use to integrate your application with AElf, similar to web3.js for Ethereum.

These libraries allow you to interact with a local or remote aelf node, using a HTTP connection.

The following documentation will guide you through installing and running the libraries, as well as providing an API reference documentation with examples.

If you need more information, you can check out the repos:

- Javascript: https://github.com/AElfProject/aelf-web3.js
- C#: https://github.com/AElfProject/aelf-sdk.cs
- Go: https://github.com/AElfProject/aelf-sdk.go
- Java: https://github.com/AElfProject/aelf-sdk.java
- PHP: https://github.com/AElfProject/aelf-sdk.php
- Python: https://github.com/AElfProject/aelf-sdk.py

### What you'll need

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs groupId="programming-languages">
  <TabItem value="javascript" label="Javascript">

```bash
npm install aelf-sdk
```

  </TabItem>
  <TabItem value="dotnet" label="C#">

Package Manager:

```bash
Install-Package AElf.Client
```

.NET CLI

```bash
dotnet add package AElf.Client
```

PackageReference

```xml
<PackageReference Include="AElf.Client" Version="X.X.X" />
```

  </TabItem>
  <TabItem value="go" label="Go">

```bash
go get -u github.com/AElfProject/aelf-sdk.go
```

  </TabItem>
  <TabItem value="java" label="Java">

https://mvnrepository.com/artifact/io.aelf/aelf-sdk

```xml
<dependency>
    <groupId>io.aelf</groupId>
    <artifactId>aelf-sdk</artifactId>
    <version>0.X.X</version>
</dependency>
```

  </TabItem>
  <TabItem value="php" label="PHP">

```bash
composer require aelf/aelf-sdk dev-dev
```

  </TabItem>
  <TabItem value="python" label="Python">

```bash
pip install aelf-sdk
```

  </TabItem>

</Tabs>
