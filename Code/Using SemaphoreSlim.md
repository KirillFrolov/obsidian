---
tags:
  - "#csharp"
---
<span style="font-size:12px; color:#888888;">Created: 15.10.2024 12:46</span>


```csharp 
private static readonly SemaphoreSlim SemaphoreSlim = new SemaphoreSlim(1, 1);

await SemaphoreSlim.WaitAsync();  
try  
{  
    //some async code
}  
finally  
{  
    SemaphoreSlim.Release();  
}
```