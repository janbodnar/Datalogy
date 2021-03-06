# Groovy

## Download page & print to console 

```groovy
def url = 'https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019'
println url.toURL().text
```

## Download page & write to file

```groovy
def fileName = new File('index.html') 
def url = 'https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019'

fileName << new URL(url).text
```

## Parse HTML table from URL

```groovy
@Grab(group='org.jsoup', module='jsoup', version='1.10.1')

import org.jsoup.Jsoup

def url = 'https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019'

def doc = Jsoup.connect(url).get()
def e = doc.getElementById('stores-list--section-16266')

println e.child(0)
```


## Parse HTML table from a file 

```groovy
@Grab(group='org.jsoup', module='jsoup', version='1.10.1')

import org.jsoup.Jsoup

def doc = Jsoup.parse(new File('index.html'), 'utf-8')
def e = doc.getElementById('stores-list--section-16266')

println e.child(0)
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
	
    def res = tr.getElementsByTag('td').drop(2).take(1).first().text()
    vals.add(ci.parse(res).toBigDecimal())	
}

def sum = vals.sum()
def avg = vals.average()

println "The sum is ${sum}"
println "The average is ${avg}"
```

## Top ten & bottom ten rows

```groovy
@Grab('org.jsoup:jsoup:1.10.1')

import org.jsoup.Jsoup

def url = 'https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019'

def doc = Jsoup.connect(url).get()
def e = doc.getElementById('stores-list--section-16266')

def table = e.child(0)

println "Top 10"

def trs = table.getElementsByTag('tr').drop(1).take(10)

def vals = []

for (tr in trs) {
    
    def val = []
    def res = tr.getElementsByTag('td').take(3).forEach {
        val.add(it.text())
    }
    vals.add(val)
}

for (val in vals) {
    println val
}

println '----------------------------------------------'

println "Bottom 10"

def trs2 = table.getElementsByTag('tr').drop(1).takeRight(10)

def vals2 = []

for (tr in trs2) {

    def val = []
    def res = tr.getElementsByTag('td').take(3).forEach {
        val.add(it.text())
    }
    vals2.add(val)
}

for (val in vals2) {
    println val
}
```


## Show data in console table 

```groovy
@Grab('org.jsoup:jsoup:1.10.1')
@Grab('de.vandermeer:asciitable:0.3.2')

import org.jsoup.Jsoup
import de.vandermeer.asciitable.AsciiTable

def url = 'https://nrf.com/resources/top-retailers/top-100-retailers/top-100-retailers-2019'

def doc = Jsoup.connect(url).get()
def e = doc.getElementById('stores-list--section-16266')

def table = e.child(0)

def headers = []
def ths = table.getElementsByTag('th').toList().take(3).forEach {
    headers.add(it.text())
}

def trs = table.getElementsByTag('tr').drop(1)
def vals = [] 

for (tr in trs) {
    def row = []
    def res = tr.getElementsByTag('td').toList().take(3).forEach {
        row.add(it.text())	
    }
    vals.add(row)
}

def ascTable = new AsciiTable()
ascTable.addRule()

ascTable.addRow(headers)
ascTable.addRule()

for (row in vals) {
    ascTable.addRow(row)
}

ascTable.addRule()

println ascTable.render()
```

## Export into CSV file

## Export into Excel file




