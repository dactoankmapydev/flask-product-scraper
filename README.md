# Flask Microservice - Product Scraper
A simple web-scraper and microservice for processing categories/products,
written in [Python/Flask](http://flask.pocoo.org).

The scraper itself has been specifically written to scrape [http://www.farah.co.uk], using the Python library [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/#Download). It scrapes category and product information into JSON files.

The Flask microservice loads the JSON data into a SQLite database and makes that data available. Please see the documentation below for more details.

## Contents
- Flask Microservice - Product Scraper
  * [Dependencies](#dependencies)
  * [Installation](#installation)
    + [Install virtualenv](#install-virtualenv)
    + [Install productscraper](#install-scraper)
    + [Run the Web Scraper](#run-scraper)
    + [Initialise the SQLite database](#init-db)
  * [Running the Services](#running-the-services)
  * [API Docs](#api-docs)
    + [Get Products](#get-products)
    + [Get Products by Category ID](#get-products-by-category-id)
    + [Get Categories](#get-categories)
  * [License](#license)

<a name="dependencies"></a>
## Dependencies
This project was built using **Python v2.7.10** and **Flask v0.12**, on **Mac OS 10.12.2**.
Please be aware that some of these instructions are platform specific and you
may need to adapt for your own platform. Please refer to the [Flask](http://flask.pocoo.org)
documentation.

<a name="installation"></a>
## Installation
<a name="install-virtualenv"></a>
### Install virtualenv
It is recommended that you run Python projects inside [virtual environments](https://virtualenv.pypa.io/en/stable/).
```
$ sudo pip install virtualenv
```

To create a virtualenv:
```
$ cd flask-product-scraper-master
$ virtualenv venv
New python executable in venv/bin/python
Installing setuptools, pip............done.
```

When you want to work on the project:
```
$ . venv/bin/activate
```

Frequent use:
```
alias venv='. venv/bin/activate'
```

To finish:
```
$ deactivate
```

<a name="install-scraper"></a>
### Install productscraper
The following will install the productscraper applications the required dependencies,
and then export the Flask app.
```
$ pip install -e .
$ export FLASK_APP=productscraper
...
```
**Next: You will need to run the web scraper to populate the database,
before you can do anything else.**

<a name="run-scraper"></a>
### Run the Web Scraper
Run the web scraper. This will take some time while it crawls the entire site and
parses the available products. Make sure you are in the same directory as `scrape.py`.
```
$ cd productscraper/
$ python scrape.py
...
```
This does two things; downloads a JSON file of all the categories to database/category.json
and downloads a JSON file of all the products to database/product.json.
The list of URLs that are crawled will be printed to the console.

<a name="init-db"></a>
### Initialise the SQLite database
Make sure you are in the same directory as `app.py`.
```
$ sqlite3 /tmp/productscraper.db < database/schema.sql
$ flask initdb
Initialized the database.
```
The database directory should now contain a `productscraper.db` file which will contain
two tables; a product table and a category table, both containing the data from
the JSON files.

<a name="running-the-services"></a>
## Running the Services
Start the app:
```
$ flask run
* Serving Flask app "productscraper"
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
...
```
If you navigate to the URL, you should see a simple UI to view the scraped products.

<a name="api-docs"></a>
## API Docs
<a name="get-products"></a>
### Get Products
* URL: /product
* Method: `GET`
* Params: none
* Success Response: `{ "catId": 1, "prodCode": "F4RF6002GP", "prodId": 1 ... }`
* Error Response:
* Sample call:
```javascript
$.ajax({
	url: "/product",
	dataType: "json",
	type : "GET",
	success : function(r) {
		console.log(r);
	}
});
```

<a name="get-products-by-category-id"></a>
### Get Products by Category ID
* URL: /product/category/{categoryID}
* Method: `GET`
* Params: `categoryID=integer`
* Success Response: `{ "catId": 1, "prodCode": "F4RF6002GP", "prodId": 1 ... }`
* Error Response:
* Sample call:
```javascript
$.ajax({
	url: "/product/category/",
	params: { categoryId: 1 },
	dataType: "json",
	type : "GET",
	success : function(r) {
		console.log(r);
	}
});
```

<a name="get-categories"></a>
### Get Categories
* URL: /category
* Method: `GET`
* Params: None
* Success Response: `{ "catId": 1, "parentCatId": 0, "catTitle": "Clothing" ... }`
* Error Response:
* Sample call:
```javascript
$.ajax({
	url: "/category",
	dataType: "json",
	type : "GET",
	success : function(r) {
		console.log(r);
	}
});
```

<a name="license"></a>
## License
[MIT](http://link.com)
