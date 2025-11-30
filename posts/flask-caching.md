---
title: "Simple Caching in a Flask application"
date: "2025-11-30"
---

# Caching

Caching refers to storing responses in a temporary storage area and then retrieving responses from this more convenient area instead of going back to the origin to process the request. It is of particular use when the server has to do an expensive computation in order to return a response. If the result will be the same each time, there is no need to use up resources each time a response to that request is needed. Instead, the expensive computation is done once and then stored in the cache, usually in-memory. The next time the same request is made, the response is taken from the cache which results in a better user experience.

This can be achieved in a **Flask** application using the **Flask-Caching** extension

## Installation

```py
pip install Flask-Caching
```

## Usage

A cache object is used to manage the cache.

The **Cache** class is imported from **flask_caching** which was installed in the previous step.  
The cache type was set to [SimpleCache](https://flask-caching.readthedocs.io/en/latest/#simplecache)

### app.py

```py
from flask import Flask
from flask_restful import Api
from flask_caching import Cache

app = Flask(__name__)
api = Api(app)

app.config['CACHE_TYPE'] = 'SimpleCache'
cache = Cache(app)


from stocks.stock_list import StocksList

api.add_resource(StocksList, '/api/stocks')

if __name__ == '__main__':
    app.run(debug=True)
```

Having created the cache instance in the **app.py** file, we can now import it into our **stocklist.py** file to cache the desired endpoints.

We import it and then decorate the endpoint using the **@cache.cached** annotation

### stocklist.py

```py
# ...
# other imports

from app import cache

class StocksList(Resource):
    #...
    # other class details

    @cache.cached(3600)
    def get(self):
        data = self.do_expensive_computation()

        return data
```

In our case, the expensive operation is **self.do_expensive_computation()**.  
The cache is given a timeout of 1 hour.  
That is, once the request is made the first time, the response will be available in memory for the next hour.  
The first request made to the endpoint after the hour has expired will result in the expensive computation being done again.  
It will then be cached once more.
