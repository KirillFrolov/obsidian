---
tags:
  - nuget
---
<span style="font-size:12px; color:#888888;">Created: 11.11.2024 11:32</span>

```shell 
nuget push .\Pilot.SDK.Extensions.1.0.0.nupkg -Source qwerta
```

 В данном случае qwerta -  source прописанный в файле NuGet.Config

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />
    <add key="qwerta" value="https://gitlab.com/api/v4/projects/45847473/packages/nuget/index.json" />
  </packageSources>
  
  <packageSourceCredentials>
    <qwerta>
        <add key="Username" value="qwerta" />
        <add key="ClearTextPassword" value="odRYBMjhTzEsz1ia6zf2" />
      </qwerta>
  </packageSourceCredentials>
</configuration>

``` 