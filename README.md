![](http://i.imgur.com/wYi2CkD.png)

# ABOUT

[![Build Status](https://travis-ci.org/jeanralphaviles/comment_parser.svg?branch=master)](https://travis-ci.org/jeanralphaviles/comment_parser/branches)
[![PyPI status](https://img.shields.io/pypi/status/comment_parser.svg)](https://pypi.python.org/pypi/comment_parser/)
[![PyPI version shields.io](https://img.shields.io/pypi/v/comment_parser.svg)](https://pypi.python.org/pypi/comment_parser/)
[![PyPI license](https://img.shields.io/pypi/l/comment_parser.svg)](https://pypi.python.org/pypi/comment_parser/)
[![PyPI pyversions](https://img.shields.io/pypi/pyversions/comment_parser.svg)](https://pypi.python.org/pypi/comment_parser/)

Installation
-------------

::

    pip install parsechain // In developing //


Usage
-----

.. code:: python

    import requests
    from parseshuttle import C, Response

    # Fetch html and cast it
    response = Response.cast(requests.get(...))

    # Get a movie title and rating
    title = response.css('h1 .title').text
    rating = response.css('.left-box').inner_text.re(r'IMDb: ([\d.]+)').float()

    # Or both
    movie = response.root.multi({
        'title': C.css('h1 .title').text,
        'rating': C.css('.left-box').inner_text.re(r'IMDb: ([\d.]+)').float(),
    })


The last example could be extended to show chains reuse:


.. code:: python

    def by_label(label):
        return C.css('.left-box').inner_text.re(fr'{label}: ([\w.]+)')

    parse_movie = C.multi({
        'title': C.css('h1 .title').text,
        'rating': by_label('IMDb').float,
        'status': by_label('Status').trim,
    })

    movie = parse_movie(response.root)  # Pass a root of a tree


The complete list of available ops could be seen in ``parsechain.chains.Ops`` class. Proper documentation to follow, some day ;)
