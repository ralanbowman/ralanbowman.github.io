---
title: Documentation for the Uses This API
sidebar: second_sidebar
toc: false
permalink: uses-this-api.html
---

[The Setup](https://usesthis.com "The Setup/Uses This") is a website with the tagline “What do people use to get stuff done?” The site publishes interviews with people mainly from the tech/startup/artist crowd, asking what equipment and software and other gear they use to do their jobs or pursue their hobbies. 

The website has a simple RESTful API with requests and responses in JSON. There is [minimal documentation](https://usesthis.com/api/ "Uses This API") for the API, so I updated the documentation on my own as an exercise in creating simple but useful documentation for a RESTful API. 

**Note** - I am not affiliated with the website or its author, just a fan who has been reading the site almost since its launch. 

---

# The Setup API 

A simple JSON API to retrieve data from [The Setup/Uses This](https://usesthis.com "The Setup/Uses This"). 

[Retrieve a list of all interviews](#retrieve-a-list-of-all-interviews)  
[Retrieve a specific interview, including gear](#retrieve-a-specific-interview-including-gear)  
[Retrieve a list of all categories](#retrieve-a-list-of-all-categories)  
[Retrieve a list of all interviews in a category](#retrieve-a-list-of-all-interviews-in-a-category)  

[Retrieve a list of all gear (hardware or software)](#retrieve-a-list-of-all-gear-hardware-or-software)  
[Retrieve a list of all gear (hardware or software) for a category](#retrieve-a-list-of-all-gear-hardware-or-software-for-a-category)  

[Retrieve the top 50 hardware or software items listed (can also specify year)](#retrieve-the-top-50-hardware-or-software-items-listed-can-also-specify-year)  
[Retrieve site stats for number of interviews, hardware and software item counts](#retrieve-site-stats-for-number-of-interviews-hardware-and-software-item-counts)  

[Status Codes and Errors](#status-codes-and-errors)  


## Interviews

## Retrieve a list of all interviews

Returns a basic list of all interviews currently available.

**Note** - this will return a very long list of items.

### Endpoint

`/api/v1/interviews`  

### Method and URL

`GET https://usesthis.com/api/v1/interviews`  

### Sample Request

`curl -X GET "https://usesthis.com/api/v1/interviews/"`

### Sample Response

```json
{
  "interviews": [
    {
      "slug": "nina.freeman",
      "name": "Nina Freeman",
      "url": "http://usesthis.com/interviews/nina.freeman/",
      "summary": "Level designer (Fullbright), game designer (Cibele)",
      "date": 1458198000,
      "categories": [
        "developer",
        "designer",
        "game",
        "mac",
        "windows"
      ]
    },
    {
      "slug": "megan.prelinger",
      "name": "Megan Prelinger",
      "url": "http://usesthis.com/interviews/megan.prelinger/",
      "summary": "Writer, historian, librarian, naturalist",
      "date": 1458025200,
      "categories": [
        "historian",
        "librarian",
        "mac",
        "naturalist",
        "writer"
      ],
      "credits": {
        "name": "Rick Prelinger (© 2014)"
      }
    }
  ]
}
```

| Element     |   Description   |   Type   |   Notes   |
|-------------|-----------------|----------|-----------|
|  **interviews**  |  Basic interview information   |   JSON array   |  |
|  **slug**  |    Page ID    |  string   |  In "first.last" name format    |
|  **name**  |    Full name  |  string   |  First and last name    |
|  **url**  |    Link to full interview        |  string   |  |
|  **summary**  |    Summary of the interview        |  string   |  |
|  **date**  |    Date interview was posted  |  integer   |  **date** is in UNIX epoch time    |
|  **categories**  |    List of categories for the interview  |  JSON array   |  |
|  **credits**  |    Photo credit for interview picture    |  JSON object   |  Credit is only listed when provided by subject    |
|  **name**  |    Name of photographer        |  string   |  &nbsp;    |

<br />

## Retrieve a specific interview, including gear

Return a specific interview for a given *slug*, including gear (hardware and software used).

### Endpoint

`/api/v1/interviews/{slug}`

**Note** - a ***slug*** is a short label for an article that is used as part of a URL, usually derived from the title. See [**Retrieve a list of all interviews**](#retrieve-a-list-of-all-interviews) to get a full list of available slugs.

### Method and URL

`GET https://usesthis.com/api/v1/interviews/{slug}`


### Sample Request

`curl -X GET "https://usesthis.com/api/v1/interviews/jessamyn.west/"`


### Sample Response

```json
{
    "interview": {
        "slug": "slug.name",
        "name": "Slug Name",
        "url": "http://usesthis.com/interviews/slug.name/",
        "summary": "maker, programmer",
        "date": 1458025200,
        "categories": [
            "maker",
            "programmer",
            "mac",
            "linux",
        ],
        "credits": {
            "name": "Photographer (© 2014)"
        },
        "contents": "#### Who are you, and what do you do?\n\n[I'm](http://example.com/ \"Example's website.\") a maker, and a programmer... (output truncated)",
    "gear":{
      "hardware":[
        {
          "slug":"macbook-air",
          "name":"Macbook Air",
          "description":"A very thin laptop.",
          "url":"http://www.apple.com/macbook-air/"
        },
        {
          "slug":"imac",
          "name":"iMac",
          "description":"An all-in-one computer.",
          "url":"http://www.apple.com/imac/"
        },
      ]
    }
  }
}
```


| Element     |   Description   |   Type   |   Notes   |
|-------------|-----------------|----------|-----------|
|  **interviews**  |  Basic interview information   |   JSON array   |  |
|  **slug**  |    Page ID    |  string   |  In "first.last" name format    |
|  **name**  |    Full name  |  string   |  First and last name    |
|  **url**  |    Link to full interview        |  string   |  |
|  **summary**  |    Summary of the interview        |  string   |  |
|  **date**  |    Date interview was posted  |  integer   |  **date** is in UNIX epoch time    |
|  **categories**  |    List of categories for the interview  |  JSON array   |  |
|  **credits**  |    Photo credit for interview picture    |  JSON object   |  Credit is only listed when provided by subject    |
|  **name**  |    Name of photographer        |  string   |  &nbsp;    |
|  **contents**  |    Interview contents, in Markdown format  |  string   |  &nbsp;    |
|  **gear**  |    List of hardware and software  |  JSON array   |  &nbsp;  |
|  **hardware**  |  Hardware used      |  JSON object   |  &nbsp;    |
|  **software**  |    Software used    |  JSON object   |  &nbsp;    |


<br />

## Retrieve a list of all categories

Returns a basic list of all categories currently available

**Note** - this will return a long list of items.

### Endpoint

`/api/v1/categories`

### Method and URL

`GET https://usesthis.com/api/v1/categories`


### Sample Request

`curl -X GET "https://usesthis.com/api/v1/categories/"`


### Sample Response

```json
{
  "categories": [
    "activist",
    "actor",
    "adventurer",
    "agriculture",
    "animal",
    "animator",
    "anthropologist",
    "architect",
    "artist",
    "astrologer",
    "output truncated..."
   ]
}
```


| Element     |   Description   |   Type   |   Notes   |
|-------------|-----------------|----------|-----------|
|  **categories**  |    List of categories        |  JSON array   |  &nbsp;    |

<br />

## Retrieve a list of all interviews in a category

Returns a list of all interviews available in a specified category.

### Endpoint

`/api/v1/categories/{slug}`

**Note** - a ***slug*** is a short label for a category that is used as part of a URL. See [**Retrieve a list of all categories**](#retrieve-a-list-of-all-categories) to get a full list of available slugs.

### Method and URL

`GET https://usesthis.com/api/v1/categories/{slug}`


### Sample Request

`curl -X GET "https://usesthis.com/api/v1/categories/mac/"`


### Sample Response

```json
{
  "interviews": [
    {
      "slug": "first.last",
      "name": "Firstname Lastname",
      "url": "http://usesthis.com/interviews/slug.name/",
      "summary": "Summary of interview subject",
      "date": 1410962400,
      "categories": [
        "category1",
        "category2",
        "category3",
        "category4"
      ]
    }
  ]
}
```

| Element     |   Description   |   Type   |   Notes   |
|-------------|-----------------|----------|-----------|
|  **interviews**  |  Basic interview information   |   JSON array   |  |
|  **slug**  |    Page ID    |  string   |  In "first.last" name format    |
|  **name**  |    Full name  |  string   |  First and last name    |
|  **url**  |    Link to full interview        |  string   |  |
|  **summary**  |    Summary of the interview        |  string   |  |
|  **date**  |    Date interview was posted  |  integer   |  **date** is in UNIX epoch time    |
|  **categories**  |    List of categories for the interview  |  JSON array   |  |

<br />

---

## Gear Listings

## Retrieve a list of all gear (hardware or software)

Returns a list of all gear currently available, either hardware or software.

**Note** - these lists will be extremely long. 

### Endpoint

For hardware:  
`/api/v1/hardware`

For software:  
`/api/v1/software`  

### Method and URL

For hardware:  
`GET https://usesthis.com/api/v1/hardware/`

For software:  
`GET https://usesthis.com/api/v1/software/`

### Sample Request

For hardware:  
`curl -X GET  "https://usesthis.com/api/v1/hardware/"`

For software:  
`curl -X GET  "https://usesthis.com/api/v1/software/"`


### Sample Response
(Same response format for either `hardware` or `software`)

```json
{
  "gear": [
    {
      "slug": "01v",
      "name": "01V"
    },
    {
      "slug": "055xprob",
      "name": "055XPROB"
    },
    {
      "slug": "1",
      "name": 1
    },
  ]
}
```

| Element     |   Description   |   Type   |   Notes   |
|-------------|-----------------|----------|-----------|
|  **gear**  |    List of gear        |  JSON array   |  &nbsp;    |
|  **slug**  |    Page ID    |  string   |  Will be in "dashed-word-format" if more than one word   |
|  **name**  |    Gear name        |  string   |  Will be a proper name    |

<br />


## Retrieve a list of all gear (hardware or software) for a category 

Returns a list of all interviews for a specific piece of hardware or software.

### Endpoint

For hardware:  
`/api/v1/hardware/{slug}`

For software:  
`/api/v1/software/{slug}`  

**Note** - a ***slug*** is a short label for an article that is used as part of a URL, usually derived from the title. See [**Retrieve a list of all gear (hardware or software)**](#retrieve-a-list-of-all-gear-hardware-or-software) to get a full list of available slugs.

### Method and URL

For hardware:  
`GET https://usesthis.com/api/v1/hardware/{slug}`

For software:  
`GET https://usesthis.com/api/v1/software/{slug}`

### Sample Request

For hardware:  
`curl -X GET  "https://usesthis.com/api/v1/hardware/iphone/"`

For software:  
`curl -X GET  "https://usesthis.com/api/v1/software/ios"`


### Sample Response
(Same response format for either `hardware` or `software`)


```json
{
  "gear": {
    "slug": "ios",
    "name": "iOS",
    "description": "A mobile operating system.",
    "url": "http://www.apple.com/ios/",
    "interviews": [
      {
        "slug": "rachel.hyman",
        "name": "Rachel Hyman"
      }
    ]
  }
}
```

| Element     |   Description   |   Type   |   Notes   |
|-------------|-----------------|----------|-----------|
|  **gear**  |    List of gear   |  JSON object   |  &nbsp;    |
|  **slug**  |    Page ID    |  string   |   Will be in "dashed-word-format" if more than one word   |
|  **name**  |    Gear name    |  string   |  Will be a proper name    |
|  **description**  |   Short description     |  string   |  &nbsp;    |
|  **url**  |    Vendor website    |  string   |  &nbsp;    |
|  **interviews**  |    All related interviews  |  JSON array   |  &nbsp;    |
|  **slug**  |    Page ID   |  string   |  In "first.last" name format    |
|  **name**  |    Full name        | string   |  First and last name    |

<br />

---

## Site Statistics 

## Retrieve the top 50 hardware or software items listed (can also specify year)

Returns a list of the top 50 hardware or software items listed on the site, with counts for each item. The year can also be specified to show counts for that year. 

### Endpoint

For hardware:  
`/api/v1/stats/hardware`

For software:  
`/api/v1/stats/software`

**To show the year:**

For hardware:  
`/api/v1/stats/hardware/YYYY/`

For software:  
`/api/v1/stats/software/YYYY/`

### Method and URL

For hardware:  
`GET https://usesthis.com/api/v1/stats/hardware`

For software:  
`GET https://usesthis.com/api/v1/stats/software`

### Sample Request

For hardware:  
`curl -X GET https://usesthis.com/api/v1/stats/hardware/`

For software:  
`curl -X GET https://usesthis.com/api/v1/stats/software/`

### Sample Response
(Same response format for either `hardware` or `software`)

```json
{
  "gear": [
    {
      "slug": "photoshop",
      "count": 285
    },
    {
      "slug": "chrome",
      "count": 209
    },
    {
      "slug": "dropbox",
      "count": 195
    }
  ]
}
```


| Element     |   Description   |   Type   |   Notes   |
|-------------|-----------------|----------|-----------|
|  **gear**  |    List of gear    |  JSON array   |  &nbsp;    |
|  **slug**  |    Page ID        |  string   |  Will be in "dashed-word-format" if more than one word    |
|  **count**  |    Number of times gear is mentioned  |  integer   |  &nbsp;    |

<br />

## Retrieve site stats for number of interviews, hardware and software item counts

Returns a count of the current number of interviews posted, and the number of hardware and software items counted. 

**Note** - these numbers will change as the site is updated. 

### Endpoint

`/api/v1/stats`

### Method and URL

`GET https://usesthis.com/api/v1/stats/`


### Sample Request

`curl -X GET https://usesthis.com/api/v1/stats/`


### Sample Response

```json
{
  "interviews": 708,
  "hardware": 2583,
  "software": 3029
}
```


| Element     |   Description   |   Type   |   Notes   |
|-------------|-----------------|----------|-----------|
|  **interviews**  | Current number of interviews   |  integer   |  &nbsp;    |
|  **hardware**  |    Current number of hardware items counted   |  integer   |  &nbsp;    |
|  **software**  |    Current number of software items counted   |  integer   |  &nbsp;    |

---

## Status Codes and Errors

|     Code    | Description     |  Notes       |
|-------------|-----------------|--------------|
|  **200**    |    OK           |  Success     |
|  **301**    |   Moved Permanently | Verify trailing slash "/" on request |
|  **404**    |    Not found    |  API endpoint not found |
