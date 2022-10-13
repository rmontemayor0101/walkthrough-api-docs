# API DOCUMENTATION

## Endpoints
* POST /api/houses
* GET /api/houses/<house_id>

### Create a listing
##### URL Endpoint
  * POST **/api/houses**

##### Parameters
* **address** - required dictionary
  * address.**line_1** - required string
    * Address line 1 (e.g., street, PO Box, or company name).
  * address.**line_2** - required string
    * Address line 2 (e.g., apartment, suite, unit, or building).
  * address.**city** - required string
    * City (ex. Denver)
  * address.**state** - required string
    * [State Abbrevation](https://www.bls.gov/respondents/mwr/electronic-data-interchange/appendix-d-usps-state-abbreviations-and-fips-codes.htm), Postal (ex. CO, AL, AK, AZ)
  * address.**zip** - required int
    * ZIP
* **sqft** - required int
  * House's square feet
* **scheduled_time** - required datetime
  * The listing schedule date
* **realtor_id** - required objectId
  * Realtor id in the walkthrough system. It is a 12-byte Field Of BSON type


## Error Return Response


```


```

## Error Codes
