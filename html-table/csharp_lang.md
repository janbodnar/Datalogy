# C#

We use the standard C# HTTP Client to make a GET request and  
AngleSharp to parse HTML data.  

## Download page & print to console 

```C#
using System;
using System.Net.Http;

using var client = new HttpClient();

var url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019";
var content = await client.GetStringAsync(url);

Console.WriteLine(content);
```

