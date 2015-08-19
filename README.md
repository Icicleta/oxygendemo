Simple Spider Task
==================

Pre-requisites
--------------
- python 2.7 (or 2.6 may suffice)
- scrapy 0.16.3 +dependencies
- pyquery

Aims
----
- crawl online retailer oxygenboutique.com for appropriate product pages
- return items representing products
- output in json format

Tests
-----
- coverage: the set of urls deemed to be appropriate product pages
- crawl efficiency: ratio of items scraped to requests made
- item particulars: the exact item contents for a subset of products

Instructions
------------
- run "scrapy startproject oxygendemo",
- update items.py in oxygendemo/oxygendemo,
- create oxygen.py in oxygendemo/oxygendemo/spiders,
- write crawling rules (these few lines are a big part of the task - you want to crawl the site in an efficient way),
   - to find appropriate category listing pages,
   - to identify individual product pages (this rule should have a callback='parse_item'),
- fill out parse_item method to populate the item's fields (one method per field),
- (import from standard python libraries where required, but nothing external other than what's already imported),
- run "scrapy crawl oxygenboutique.com -o items.json -t json",
- when satisfied, upload `oxygen.py` and `items.json` to gist and send them to me.

Examples
--------
This url: http://www.oxygenboutique.com/Ribbon-Landscape-Sweatshirt.aspx
Could yield an item dictionary:
```python
	{'code': 'Ribbon-Landscape-Sweatshirt',
	 'color': 'green',
	 'description': u"Ribbon landscape sweatshirt by clover canyon. White curved stripes add a fun ribbon effect over the floral print on this fleece lined jumper. It has a deep 'v' neckline and contrasting ribbing on the cuffs and hem. Deep 'v' neckline same day (london only within the m25) - \xa315 - same day delivery using dhl. If saturday delivery is required please call the store.",
	 'designer': 'Clover Canyon',
	 'gender': 'F',
	 'image_urls': ['http://www.oxygenboutique.com/GetImage/cT0xMDAmdz04MDAmaD02MDAmUEltZz1hOTBjOWRmNS0zNGEzLTQ3OTYtOTAwMy05ZTdkNWY0Y2RmMDguanBn0.jpg',
	                'http://www.oxygenboutique.com/GetImage/cT0xMDAmdz04MDAmaD02MDAmUEltZz00OWQxMjViYy02MGVlLTQ2NmYtODBiNC04MmQ3YmUwNzE5MzMuanBn0.jpg',
	                'http://www.oxygenboutique.com/GetImage/cT0xMDAmdz04MDAmaD02MDAmUEltZz1hYzM3NzY0ZC0yMTg4LTRmZDctOTRhMC0yODIyYmIyNzAxYTMuanBn0.jpg',
	                'http://www.oxygenboutique.com/GetImage/cT0xMDAmdz04MDAmaD02MDAmUEltZz01Nzk5ZmZhZS04NzQ4LTQ5ZTgtOTIyOS02OTlkMzM1ZTNmNTUuanBn0.jpg'],
	 'link': 'http://www.oxygenboutique.com/Ribbon-Landscape-Sweatshirt.aspx',
	 'name': u'Ribbon Landscape Sweatshirt',
	 'raw_color': u'clover',
	 'sale_discount': 70.0,
	 'stock_status': {'L': 3, 'M': 3, 'S': 3, 'XS': 1},
	 'type': 'A',
	 'usd_price': '430.91'}
```

Note that some fields are clearcut (gbp_price must equal 255.00 GBP), whereas some fields are open to interpretation (e.g. description, where arguably we could have included either more or less than in the sample here, and code, which should just be an identifier unique to this retailer.

Field details
-------------
- type, try and make a best guess, one of:
  - 'A' apparel
  - 'S' shoes
  - 'B' bags
  - 'J' jewelry
  - 'R' accessories
- gender, one of:
  - 'F' female
  - 'M' male
- designer - manufacturer of the item
- code - unique identifier from a retailer perspective
- name - short summary of the item
- description - fuller description and details of the item
- raw_color - best guess of what colour the item is (can be blank if unidentifiable)
- image_urls - list of urls of large images representing the item
- usd_price - full (non-discounted) price of the item
- sale_discount - percentage discount for sale items where applicable 
- stock_status - dictionary of sizes to stock status
  - 1 - out of stock
  - 3 - in stock
- link - url of product page