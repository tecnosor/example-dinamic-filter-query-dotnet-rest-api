# Example dinamic filter query dotnet rest api
An example of how to apply in Rest Apis with .NET a way to filter in your services with dinamic filters. It only shows the way to manage the controller, to then, apply Criteria design pattern (match criteria).

## Use case explanation

You would like to apply a dinamic filters in you Api, but you need a way to pass dinamic parameters into query to share with your GET method.

The following example is a demo that allows to your Controller of the Api get dinamic filters to apply Match Criteria Design pattern.

In the example we will using a use case that we want to search persons, where age is between 18 and 64 name contains alf and eyes color is equal to green.
The response will be order by age desc.
We want also get first 5 results.


## Curl call

```sh
curl --location -g --request GET '{{yourserverhere}}/ExampleDinamicFilterQuery?filters[0].field=age&filters[0].operator=>&filters[0].value=18&
                                                                               filters[1].field=age&filters[1].operator=<&filters[1].value=64&
                                                                               filters[2].field=name&filters[2].operator=contains&filters[2].value=alf&
                                                                               filters[3].field=eyeColor&filters[3].operator==&filters[3].value=green
&orderby=age
&order=desc
&page=1
&size=5'
```


## .NET Controller Class Implementation

```csharp

using Microsoft.AspNetCore.Mvc;

namespace tecnosor.demos.controllers
{
    [ApiController]
    [Route("[controller]")]
    public class ExampleDinamicFilterQuery : ControllerBase
    {
        public ExampleDinamicFilterQuery() { }

        [HttpGet(Name = "Test")]
        public IActionResult Get(
            [FromQuery(Name = "filters")] List<Filter> filters,
            [FromQuery(Name = "orderby")] string orderBy,
            [FromQuery(Name = "order")] string order,
            [FromQuery(Name = "page")] int page,
            [FromQuery(Name = "size")] int size)
        {
            return Ok(new
            {
                Filters = filters,
                OrderBy = orderBy,
                Order = order,
                Page = page,
                Size = size
            });
        }
        public class Filter
        {
            public string Field { get; set; }
            public string Operator { get; set; }
            public string Value { get; set; }
        }
    }
}


```

## Body Response

```json
{
    "filters": [
        {
            "field": "age",
            "operator": ">",
            "value": "18"
        },
        {
            "field": "age",
            "operator": "<",
            "value": "64"
        },
        {
            "field": "name",
            "operator": "contains",
            "value": "alf"
        },
        {
            "field": "eyeColor",
            "operator": "=",
            "value": "green"
        }
    ],
    "orderBy": "age",
    "order": "desc",
    "page": 1,
    "size": 5
}
```

## Authors
- Tecnosor
- Alfonso Soria Muñoz
