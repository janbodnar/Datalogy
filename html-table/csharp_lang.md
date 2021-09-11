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

## Download page & write to file

```C#
using System.IO;
using System.Net.Http;

using var client = new HttpClient();

var url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019";
var content = await client.GetStringAsync(url);

File.WriteAllText("index.html", content);
```

## Load HTML table 

```C#
using AngleSharp;
using System.IO;

var config = Configuration.Default.WithDefaultLoader();
using var context = BrowsingContext.New(config);

var url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019";

using var doc = await context.OpenAsync(url);

var e = doc.GetElementById("stores-list--section-16266");
var table = e.InnerHtml;

File.WriteAllText("html_table.html", table);
```
