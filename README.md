# xpath-to-excel

from lxml import html
import requests
import xlsxwriter
import datetime


now = datetime.datetime.today().strftime('%Y-%m-%d')

page = requests.get('http://econpy.pythonanywhere.com/ex/001.html')
tree = html.fromstring(page.content)

#This will create a list of buyers
buyers = tree.xpath('//div[@title="buyer-name"]/text()')

#This will create a list of prices
prices = tree.xpath('//span[@class="item-price"]/text()')

print( 'Buyers: ', buyers)
print( 'Prices: ', prices)

#creating excel sheet
workbook = xlsxwriter.Workbook('sales' + str(now) + '.xlsx')
worksheet = workbook.add_worksheet()

# Start from the first cell. Rows and columns are zero indexed.
row = 0
col = 0

#write data to excel
for index, buyer in enumerate(buyers):
    worksheet.write(row, col,     buyer)
    worksheet.write(row, col + 1, prices[index])
    row += 1

workbook.close()
