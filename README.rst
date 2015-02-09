=========================================
National Rail Enquiries Darwin Webservice
=========================================

Introduction
------------

`nre-darwin-py` provides an abstraction layer for accessing National Rail Enquiries Darwin service via their LDB SOAP web service.

This module has the following goals:

* Allow developers to avoid the complexity of SOAP and the decisions involved in choosing a Python SOAP client
* Provide a more pythonic interface to station board and service details information
* Make useful documentation available through pydoc or through the `help` command in the python shell
* Facilitate the easy creation of departure board websites and webservices

Getting started
---------------

You will need to register for Darwin access, you can do this via National Rail Enquiries `developer site <http://www.nationalrail.co.uk/46391.aspx>`_.

Basic usage
-----------

Initiate a session::

    >>> import nredarwin.webservice
    >>> darwin_sesh = nredarwin.webservice.Session(wsdl="https://lite.realtime.nationalrail.co.uk/OpenLDBWS/wsdl.aspx", api_key="YOUR_KEY")

Retrieve the departure board for Manchester Piccadilly station::

    >>> board = darwin_sesh.get_station_board('MAN')
    >>> board.location_name
    'Manchester Piccadilly'
    >> board.train_services[0].destination_text
    'Rose Hill Marple'

Retrieve more detailed information regarding a particular service::

    >>> service_id = board.train_services[0].service_id
    >>> service = darwin_sesh.get_service_details(service_id)
    >>> [cp.location_name for cp in service.subsequent_calling_points]
    [Gorton, Fairfield, Guide Bridge, Hyde Central, Woodley, Romiley, Rose Hill Marple]

The provided example.py script shows a simple departure board implementation for your reference

Practicalities
--------------

* The environment variables `DARWIN_WEBSERVICE_WSDL` and `DARWIN_WEBSERVICE_API_KEY` can be used to set the WSDL url and api key, so you don't have to specify them when initiating a Session.
* The WSDL url for the LDB Webservice may change from time to time, and has one in the past. Your application should take this into account.
* Any call to get_station_board or get_service_details will result in a query to the LDB Webservice, and therefore an HTTP request to an external service. Your application will need to handle caching and failure modes itself.
* There is an overhead involved when creating a Session, as the WSDL must be retrieved and parsed.

TODO
----

* Make departure and arrival times available as timezone-aware datetime objects
* More detailed exception handling