# DataScrapping-1

# from typing import Container
from bs4 import BeautifulSoup
import requests
import pandas as pd

for i in range(1,11):
    URL = 'https://www.walmart.com/search?q=laptop&page=' + str(i)

    HEADERS = {
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.61 Safari/537.36'
    }

    COOKIES = {
        'SPID': 'a7722903841c46f83b20e9e99bdcfd89c6eeddfadfa41dd69f6d0204178a87151fa4ef5c80bf6e18459d200bed995a45wmjet'
    }

    respond = requests.get(URL, cookies=COOKIES, headers=HEADERS)
    # respond = requests.get(URL)
    # print(respond.text)
    data = respond.content

    soup =BeautifulSoup(data, 'html.parser') 
    # print(soup)

    # soup = soup.prettify

    container = soup.find_all('div', class_='flex flex-wrap w-100 flex-grow-0 flex-shrink-0 ph2 pr0-xl pl4-xl mt0-xl mt3')
    # print(container)
    # # print= BeautifulSoup

    product_title = []
    product_link = []


    for item in container:
        #print(item)
        products = item.find_all('a', class_='absolute w-100 h-100 z-1')
        # print(products)
        for product in products:
            #print(product.prettify())
            product_name = product.find('span', class_='w_Dz').text
            products_url = product.get('href')
            # print(product_name)
            # print(products_url)
            product_title.append(product_name)
            product_link.append(products_url)

columns = ['product name', 'product_link']
df = pd.DataFrame({'product name':product_title, 'product_link':product_link}, columns=columns, index=None)
print(df)
