# ahapi
Asynchronous HTTP API Server for Python 3.5+

Allows you to spin up a simple JSON/formdata capable API server where each 
file in the `api_dir` is an endpoint. formdata or JSON payloads to the API 
endpoints are automatically converted into dictionaries/lists by the server,
and a global state object can be passed to each request.

Main server example:
~~~python3
import ahapi
import asyncio

httpserver = ahapi.simple(
    api_dir="/foo/bar/scripts", 
    bind_ip="127.0.0.1", 
    bind_port="8080", 
    state={"something": "stateful"}
    )

loop = asyncio.get_event_loop()
loop.run_until_complete(httpserver.loop())
~~~

`/foo/bar/scripts/example.py`:
~~~python3
import ahapi
import typing
""" Generic endpoint reachable at http://localhost:8080/example"""

async def process(state: typing.Any, request, formdata: dict) -> dict:
    # do_stuff_here()
    return {
        "some": "json_response",
        "global_state": state,
    }


def register(state: typing.Any):
    return ahapi.endpoint(process)

~~~

## Installing
Use pip to install it, either via `requirements.txt` or via the CLI:
~~~bash
pip install ahapi
~~~

## Serving static content
You can also serve static content from a separate directory, which will mix in with the api handlers:
~~~python3
import ahapi
import asyncio

httpserver = ahapi.simple(
    api_dir="/foo/bar/scripts",    # serve api end points from here
    static_dir="/foo/bar/htdocs",  # serve stuff like html and images from here
    bind_ip="127.0.0.1", 
    bind_port="8080", 
    state={"something": "stateful"}
    )

loop = asyncio.get_event_loop()
loop.run_until_complete(httpserver.loop())
~~~
