# Web-Scrape
This program uses Python and lxml library to scrape data from a Jumia website. 
#first import 
import requests
import lxml.html

#second we acquire and request link of the website to extract data

url= requests.get ("https://www.jumia.co.ke/styling-products-deodorant-antiperspirants/")
doc= lxml.html.fromstring(url.content)

#Third, we inspect the source website to understand the html code to use here as path
#This code returns all//paths with id under div, class with name -paxs row _no-g _4cl-3cm-shs
#[0] indicates that only one path has this name

deos= doc.xpath('//div[@class="-paxs row _no-g _4cl-3cm-shs"]')[0]

#we are in the correct path of desired items, now we get product names, new prices, old price, reviews

item_name = deos.xpath('.//h3[@class="name"]/text()')
new_price = deos.xpath('.//div[@class="prc"]/text()')
old_price = deos.xpath('.//div[@class="old"]/text()')
reviews = deos.xpath('.//div[@class="rev"]/text()')

#we have extracted the necessary data, we now combine everything together, each item with name, price, old price, reviews and place it to
output= []

for info in zip(item_name, new_price, old_price, reviews):
    resp= {}
    resp['item_name'] = info[0]
    resp['new_price'] = info[1]
    resp['old_price'] = info[2]
    resp['reviews'] = info[3]
    output.append(resp)
print(output)
