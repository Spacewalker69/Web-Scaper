from bs4 import BeautifulSoup
import re
import requests
import pandas as pd
def get_data(a):
    data=[]
    for i in a:
        data.append(i.string)
    return data
base_url='https://www.flipkart.com'
url ='https://www.flipkart.com/air-conditioners/pr?sid=j9e%2Cabm%2Cc54&otracker=categorytree&otracker=nmenu_sub_TVs+%26+Appliances_0_LG'
res=requests.get(url).text
doc=BeautifulSoup(res,"html5lib")
pages=[]
links_data=doc.find_all('a',class_='ge-49M',href=True)
for i in links_data:
    pages.append(base_url+(i.get('href')))
products=[]
prices=[]
ratings=[]
brand=[]
temp_products=[]
temp_prices=[]
temp_ratings=[]
temp_brand=[]
for link in pages:
    html_page = requests.get(link).text
    doc = BeautifulSoup(html_page, "html5lib")
    products_data = (doc.find_all("div", class_="_4rR01T"))
    temp_products = get_data(products_data)
    products.extend(temp_products)
    prices_data = (doc.find_all("div", class_="_30jeq3 _1_WHN1"))
    temp_prices = get_data(prices_data)
    prices.extend(temp_prices)
    rating_data = (doc.find_all("div", class_="_3LWZlK",limit=len(temp_products)))
    for i in rating_data:
        ratings.append(i.contents[0])
    #ratings.extend(temp_ratings)
    for i in temp_products:
        res = re.findall('([a-zA-Z ]*)\d*.*', i)
        res=str(res[0])
        brand.append(res)
    df = pd.DataFrame({'Brand':brand,'Product Name': products, 'Prices': prices, 'Rating': ratings})
    print(df)
df.to_csv('Fetched ACs Data.csv')
