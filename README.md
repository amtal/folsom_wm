### Folsom Webmachine Interface

This is an HTTP+JSON interface to https://github.com/amtal/folsom/. 

#### Building and running

You need a (preferably recent) version of Erlang installed but that should be it.

       ./rebar get-deps
       ./rebar compile

* Note that ibrowse is only a dep for the test suite.

By default folsom_wm will listen on localhost, port 5565 and log to 'priv/log'. All three can be adjusted by changing environment variables FOLSOM_IP, FOLSOM_PORT and FOLSOM_LOG_DIR.

#### Metrics API

folsom_metrics.erl is the API module you will need to use most of the time.

Retreive a list of current installed metrics:

      > folsom_metrics:get_metrics().

      $ curl http://localhost:5565/_metrics

Query a specific metric:

      > folsom_metrics:get_metric_value(Name).

      $ curl http://localhost:5565/_metrics/name


##### Erlang VM

folsom also produces Erlang VM statistics.

The result of `erlang:memory/0`:

       $ curl http://localhost:5565/_memory

The result of `erlang:system_info/1`:

       $ curl http://localhost:5565/_system

The result of `erlang:statistics/1`:

       $ curl http://localhost:5565/_statistics

#### Sample Output

Stats for metric 'a':

      $ curl http://localhost:5565/_metrics/a

      {"min":1,"max":1000,"mean":322.2,"median":100,"variance":185259.19999999998,"standard_deviation":430.4174717643325,"skewness":1.2670136514902162,"kurtosis":-1.2908313302242205,"percentile":{"75":500,"95":1000,"99":1000,"999":1000},"histogram":{"10":2,"20":0,"30":0,"50":0,"100":1,"200":0,"300":0,"400":0,"500":1,"1000":1,"99999999999999":0}}

Results of history metric 'test':

       $ curl http://localhost:5565/_metrics/test

       {"1303483997384193":{"event":"asdfasdf"}}

Erlang VM memory metrics:

       $ curl http://localhost:5565/_memory

       {"total":11044608,"processes":3240936,"processes_used":3233888,"system":7803672,"atom":532137,"atom_used":524918,"binary":696984,"code":4358030,"ets":385192}

#### Testing

If this application is included alongside folsom by a rebarized parent app, folsom_wm unit tests should pass.
