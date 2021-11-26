# F#

Usage: 

- C# HTTP Client to make GET requests
- AngleSharp to parse HTML data.  
- BetterConsoles.Tables for console table output

## Download page & print to console 

```f#
using System;
using System.Net.Http;

using var client = new HttpClient();

var url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019";
var content = await client.GetStringAsync(url);

Console.WriteLine(content);
```
