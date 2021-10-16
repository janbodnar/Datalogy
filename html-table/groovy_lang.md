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

```groovy
@Grab(group='org.jsoup', module='jsoup', version='1.10.1')
import org.jsoup.Jsoup

def doc = Jsoup.parse(new File('index.html'), 'utf-8')
def table = doc.getElementById('stores-list--section-16266')

println table.child(0)
```

## Calculate sum & average

```groovy

@Grab(group='org.jsoup', module='jsoup', version='1.10.1')
import org.jsoup.Jsoup
import java.text.NumberFormat

def url = 'https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019'

def doc = Jsoup.connect(url).get()
def e = doc.getElementById('stores-list--section-16266')

def table = e.child(0)
def trs = table.getElementsByTag('tr').drop(1)

def ci = NumberFormat.getCurrencyInstance()
//List<BigDecimal> vals = []
def vals = []

for (tr in trs) {
	
	def res = tr.getElementsByTag('td').toList().drop(2).take(1).first().text()
	vals.add(ci.parse(res).toBigDecimal())	
}

def sum = vals.sum()
def avg = vals.average()

println "The sum is ${sum}"
println "The average is ${avg}"
```

## Top ten & bottom ten rows

## Show data in console table 


## Export into CSV file



