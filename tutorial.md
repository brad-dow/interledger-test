# Searching for jokes

We've all been there. The opportunity presents itself for a perfectly timed dad joke to provoke eye rolls and heavy sighs. The only problem is...you can't think of one. It's tough coming up with a joke in the heat of the moment!

Enter the icanhazdadjoke API! In this tutorial, we'll take a look at how to search for that perfect dad joke. By the end of this guide, you will know how to:

- Find one or more dad jokes with a simple keyword search
- Alter the response format
- Use pagination parameters to manage the response

## Search for a dad joke

Use the icanhazdadjoke API's search endpoint to retrieve a list of dad jokes that meet a specified query term. Configure the `term` parameter to the desired search criteria.

**HTTP method**: GET

**Endpoint**: /search

**Query string parameter**: `term`

In the example below, let's search for a dad joke that contains the word "write". Pay attention to the following three things we've configured in this cURL request:

- `Accept` request header is set to `application/json`
- `User-Agent` request header is set to appropriately identify who is making the request
- Search `term` value is set to "write" 

**Sample request**

```bash
$curl -H "Accept: application/json" -H "User-Agent: ILF Test (bradley.s.dow@gmail.com)" https://icanhazdadjoke.com/search?term=write
```

The JSON response for our example request contains four dad jokes in a `results` array that meet our search criteria. The response also contains the number of search results, pagination data, our original search term, and the HTTP status returned.

**Sample response**
```json
{
    "current_page": 1,
    "limit": 20,
    "next_page": 1,
    "previous_page": 1,
    "results": [
        {
            "id": "OCtWnGJtPuc",
            "joke": "Why is the new Kindle screen textured to look like paper? So you feel write at home."
        },
        {
            "id": "O7haxA5Tfxc",
            "joke": "Where do cats write notes? Scratch Paper!"
        },
        {
            "id": "EYo4TCAdUf",
            "joke": "I tried to write a chemistry joke, but could never get a reaction."
        },
        {
            "id": "SvPRCIeiNuc",
            "joke": "How does a dyslexic poet write? Inverse."
        }
    ],
    "search_term": "write",
    "status": 200,
    "total_jokes": 4,
    "total_pages": 1
}
```
### Response formats
Our request example above set the `Accept` header to `application/json` and the corresponding response was returned in JSON format.

In the example below, let's use the same request as before but instead of a JSON response, we'd rather have it in a plain text output. Change the `Accept` header to `text/plain`.

**Sample request**

```bash
$curl -H "Accept: text/plain" -H "User-Agent: ILF Test (bradley.s.dow@gmail.com)" https://icanhazdadjoke.com/search?term=write
```

Instead of the complex JSON response as before, now we have a simple, plain text response.

**Sample response**

```
How does a dyslexic poet write? Inverse.
I tried to write a chemistry joke, but could never get a reaction.
Why is the new Kindle screen textured to look like paper? So you feel write at home.
Where do cats write notes? Scratch Paper!
```

!!! note "Supported `Accept` headers"

    The icanhazdadjoke API supports the following `Accept` headers:

    - application/json
    - text/plain
    - text/html

### Pagination parameters
As the database of dad jokes continues to increase at a rapid pace, it's important to handle the search results in an elegant manner. If you query the `/search` endpoint without setting a search `term`, the response contains every dad joke available. By default, the search results are limited to 20 dad jokes per page with the first page of results being displayed.

!!! note "Maximum `limit` value
    
    The maximum value for `limit` is 30 search results.

In the example below, let's omit our search criteria to retrieve the maximum amount of dad jokes. We will set the following pagination parameters:

- `limit` is set to "5" results
- `page` is set to "2", to return the second page of 30 results

**Sample request**

```bash
$curl -H "Accept: application/json" -H "User-Agent: ILF Test (bradley.s.dow@gmail.com)" https://icanhazdadjoke.com/search?limit=5&page=2
```

Without a search criteria specified, he JSON response shows the total number of jokes as 744. Since we set a `limit` of "5", we see our five dad jokes in the `results` array. Because we set our `page` to "2", we can see that the `current_page` is "2". Since there are 744 jokes total and we are limiting our response to five jokes per page, all of the jokes would be spread across 149 pages.

**Sample response**
```json
{
    "current_page": 2,
    "limit": 5,
    "next_page": 3,
    "previous_page": 1,
    "results": [
        {
            "id": "0DdaxAX0orc",
            "joke": "I accidentally took my cats meds last night. Don’t ask meow."
        },
        {
            "id": "0DtrrOZDlyd",
            "joke": "Chances are if you' ve seen one shopping center, you've seen a mall."
        },
        {
            "id": "0L6MexPukjb",
            "joke": "Dermatologists are always in a hurry. They spend all day making rash decisions. "
        },
        {
            "id": "0LuXvkq4Muc",
            "joke": "I knew I shouldn't steal a mixer from work, but it was a whisk I was willing to take."
        },
        {
            "id": "0gFdFBsWDd",
            "joke": "I won an argument with a weather forecaster once. His logic was cloudy..."
        }
    ],
    "search_term": "",
    "status": 200,
    "total_jokes": 744,
    "total_pages": 149
}
```