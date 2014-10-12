##  pygnip - Another Python wrapper for GNIP

A Python client wrapper for the GNIP API's with optional Django integration.

This project aims to wrap *all* the API's with a single configurable client so you can seemlessly work with any stream/activity/api.

Please feel free to pull request further api integration as I have only added the API's that I've personally used so far.

Currently implemented:

* Usage API:
  * Query usage
* Search API:
  * Query, Set and Delete Powertrack rules
  * Query the search API (search and counts) 

###Configuration

This package contains a sample config file (_gnip_config.ini).

pygnip needs one of the following configuration options:
  * A gnip_config.ini file in the pygnip directory
  * A gnip_config.ini file in ~/.config
  * A correctly configured dictionary in Django settings.py (see below)

###Django integration

```
PYGNIP_CONFIG = {
    'DEFAULT': {
        'account': account,
        'username': username,
        'password': password,
        'data_source': '',
        'stream_label': '',
    },
}
```

###Usage

Create a client (by default pygnip uses 'config=DEFAULT')

```client = gnip_client.AllAPIs()```

####Query the usage API

```
client.api('UsageAPI').get_usage(bucket='',
                                 fromDate='',
                                 toDate='')
```

####Query the Search Count API

```
client.api('SearchAPICount').search_count(query='',
                                          publisher='twitter',
                                          fromDate='',
                                          toDate='',
                                          bucket=''))
```

####Query the Search API

```
client.api('SearchAPI').max_results_only(query='',
                                         publisher='twitter',
                                         fromDate='',
                                         toDate='',
                                         maxResults='10')
```

For queries which require pagination (ie repeated calls to GNIP with 'next'), you can query ```all_results()``` which returns a python list of all results from memory or alternatively the generator method ```iterate_all_results()```

```
results = client.api('SearchAPI').iterate_all_results(query='',
                                                   publisher='twitter',
                                                   fromDate='',
                                                   toDate='',
                                                   maxResults=20
                                                  )
results.next() 
for r in result:
    # do something
```

####Post a Powertrack rule

```client.api('PowertrackAPIRules').post_rule(value='test_rule', tag='test_rules')```

####Query Powertrack rules

```client.api('PowertrackAPIRu	les').get_rules()```

####Delete a Powertrack rule

```self.client.api('PowertrackAPIRules').delete_rule(value='test_rule', tag='test_rules'))```

###Handling Responses

pygnip uses the Requests library and by default decodes all json responses using the .json() method in Requests.  You can change this default behaviour by passing in 'decode_json=False' as the first argument to any API call which returns JSON.

###Tests

To run the test suite

``` python tests ```