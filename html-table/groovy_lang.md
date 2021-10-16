# Groovy

## Download page & print to console 

```groovy
def page = 'https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019'
println page.toURL().text
```

## Download page & write to file

```groovy
def fileName = new File('index.html') 
def page = 'https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019'

fileName << new URL(page).text
```

## Parse HTML table from URL

```groovy
@Grab(group='org.jsoup', module='jsoup', version='1.10.1')
import org.jsoup.Jsoup

def url = 'https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019'

def doc = Jsoup.connect(url).get()
def table = doc.getElementById('stores-list--section-16266')

println table.child(0)
```


## Parse HTML table from a file 

## Calculate sum & average

## Top ten & bottom ten rows

## Show data in console table 


## Export into CSV file




