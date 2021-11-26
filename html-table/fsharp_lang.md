# F#

Usage: 

- C# HTTP Client to make GET requests
- AngleSharp to parse HTML data.  
- BetterConsoles.Tables for console table output

## Download page & print to console 

```f#
open System.Net.Http
open System

let doWork (url:string) = 

    task {
        use client = new HttpClient()
        let! content = client.GetStringAsync(url)

        return content
    }

let url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019"
let content = doWork url
Console.WriteLine(content.Result)
```

## Download page & write to file

```f#
open System.IO
open System.Net.Http

let download2File (url:string) =

    task {
        use client = new HttpClient()
        let! content = client.GetStringAsync(url)

        return content
    }

let url =
    "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019"
let content = download2File url
File.WriteAllText("index.html", content.Result)

```



