# C#

Usage: 

- C# HTTP Client to make GET requests
- AngleSharp to parse HTML data.  
- BetterConsoles.Tables for console table output

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

## Parse HTML table from URL

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

## Parse HTML table from a file 

```C#
using System;
using System.Linq;
using System.IO;
using AngleSharp;

var html = File.ReadAllText("index.html");

var config = Configuration.Default;
using var context = BrowsingContext.New(config);
using var doc = await context.OpenAsync(req => req.Content(html));

var htable = doc.GetElementById("stores-list--section-16266");
var trs = htable.QuerySelectorAll("tr");

foreach (var tr in trs)
{
    var data = tr.QuerySelectorAll("td,th").Take(4);

    var res = (from e in data
               select e.TextContent).ToArray();

    Console.WriteLine(string.Join(" ", res));
}
```

## Calculate sum & average

```C#
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using AngleSharp;

var config = Configuration.Default.WithDefaultLoader();
using var context = BrowsingContext.New(config);

var url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019";

using var doc = await context.OpenAsync(url);

var htable = doc.GetElementById("stores-list--section-16266");

var ths = htable.QuerySelectorAll("th");
var trs = htable.QuerySelectorAll("tr").Skip(1);

var values = new List<decimal>();

foreach (var tr in trs)
{
    var val = tr.QuerySelectorAll("td").Skip(2).Take(1).First().TextContent;
    
    decimal d = decimal.Parse(val, NumberStyles.Currency);
    values.Add(d);
}

var avg = (from val in values
    select val).Average();

var sum = (from val in values
    select val).Sum();

Console.WriteLine($"The sum is: {sum}");
Console.WriteLine($"The average is: {avg}");
```

## Top ten & bottom ten rows

```C#
using System;
using System.Linq;
using AngleSharp;

var config = Configuration.Default.WithDefaultLoader();
using var context = BrowsingContext.New(config);

var url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019";

using var doc = await context.OpenAsync(url);

var htable = doc.GetElementById("stores-list--section-16266");
var trs = htable.QuerySelectorAll("tr");


Console.WriteLine("Top ten:");

foreach (var tr in trs.Skip(1).Take(10))
{
    var data = tr.QuerySelectorAll("td").Take(4);

    var res = (from e in data
               select e.TextContent).ToArray();

    Console.WriteLine(string.Join(" ", res));
}

Console.WriteLine("--------------------------------------");


Console.WriteLine("Bottom ten:");

foreach (var tr in trs.TakeLast(10))
{
    var data = tr.QuerySelectorAll("td,th").Take(4);

    var res = (from e in data
               select e.TextContent).ToArray();

    Console.WriteLine(string.Join(" ", res));
}
```

## Show data in console table 

```C#
using System;
using System.Linq;
using AngleSharp;
using BetterConsoles.Tables;

var config = Configuration.Default.WithDefaultLoader();
using var context = BrowsingContext.New(config);

var url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019";

using var doc = await context.OpenAsync(url);

var htable = doc.GetElementById("stores-list--section-16266");

var ths = htable.QuerySelectorAll("th");
var trs = htable.QuerySelectorAll("tr").Skip(1);

var headers = (from th in ths
    select th.TextContent).Take(3).ToArray();

var table = new Table(headers);

foreach (var tr in trs)
{
    var tds = tr.QuerySelectorAll("td").Take(3);
    var res = (from e in tds select e.TextContent).ToArray();

    table.AddRow(res);
}


Console.Write(table.ToString());
```

```console
$ dotnet run
------------------------------------------------------------------------   
  Rank | Company                           | 2018 retail sales (billions) 
------------------------------------------------------------------------   
 1     | Walmart                           | $387.66                      
--------------------------------------------------------------------------   
 2     | Amazon.com                        | $120.93                      
--------------------------------------------------------------------------   
 3     | The Kroger Co.                    | $119.70                      
--------------------------------------------------------------------------   
 4     | Costco                            | $101.43                      
--------------------------------------------------------------------------   
 5     | Walgreens Boots Alliance          | $98.39                       
--------------------------------------------------------------------------   
 6     | The Home Depot                    | $97.27                       
--------------------------------------------------------------------------  
...   
```

## Export into CSV file

We are using the CsvHelper library  

```C#
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using AngleSharp;
using CsvHelper;

var config = Configuration.Default.WithDefaultLoader();
using var context = BrowsingContext.New(config);

var url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019";
using var doc = await context.OpenAsync(url);

var htable = doc.GetElementById("stores-list--section-16266");
var trs = htable.QuerySelectorAll("tr").Skip(1);

var csvConfig = new CsvHelper.Configuration.CsvConfiguration(CultureInfo.CurrentCulture)
{
    ShouldQuote = args => false
};


using var fs = new StreamWriter("data.csv");
using var writer = new CsvWriter(fs, csvConfig);

var rows = new List<Row>();

foreach (var tr in trs)
{
    var tds = tr.QuerySelectorAll("td").Take(3);
    var fields = (from e in tds select e.TextContent).ToArray();
    var row = new Row(fields[0], fields[1], fields[2]);

    rows.Add(row);
}

writer.WriteRecords(rows);

record Row(string Rank, string Company, string Sales);
```

## Export into Excel file

#TODO


