# F#

Usage: 

- HTTP Client to make GET requests
- AngleSharp to parse HTML data
- FSharp.Data to maker requests & parse data
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

---

```F#
#r "nuget: FSharp.Data"

open FSharp.Data
open System

let url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019"

let content = Http.RequestString(url)
Console.WriteLine content
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

---

```F#
#r "nuget: FSharp.Data"

open FSharp.Data
open System.IO

let url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019"

let content = Http.RequestString(url)
File.WriteAllText ("index.html", content)
```

## Parse HTML table from URL

```f#
#r "nuget: AngleSharp" 

open System.IO
open AngleSharp

let parse (url:string) = 

    let config = Configuration.Default.WithDefaultLoader()
    use context = BrowsingContext.New(config)

    task {
        use! doc = context.OpenAsync(url)

        let e = doc.GetElementById("stores-list--section-16266")
        let table = e.InnerHtml

        return table
    }


let url = "https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019"
let table = parse url
File.WriteAllText("html_table.html", table.Result)
```

