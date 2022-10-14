# API DOCUMENTATION

## Endpoints
* POST /api/houses
* GET /api/houses/<house_id>

## Create a listing
*POST **/api/houses***

#### Parameters
* **address** - required dictionary
  * address.**line_1** - required string
    * Address line 1 (e.g., street, PO Box, or company name).
  * address.**line_2** - optional string
    * Address line 2 (e.g., apartment, suite, unit, or building).
  * address.**city** - required string
    * City (ex. Denver)
  * address.**state** - required string
    * [State Abbrevation](https://www.bls.gov/respondents/mwr/electronic-data-interchange/appendix-d-usps-state-abbreviations-and-fips-codes.htm), Postal (ex. CO, AL, AK, AZ)
  * address.**zip** - required int
    * ZIP
* **sqft** - required int
  * House's square feet
* **scheduled_time** - required timestamp
  * The listing schedule date. A UTC timestamp
* **realtor_id** - required string(ObjectId)
  * Realtor id in the walkthrough system. A 12-byte Field Of BSON type
* **product_type** - required enum
  * The listing's main product. Possible values are:
    * photography_package
    * photography_only
    * matterport_only
    * aerial_only
* **addons** - optional list of string
  * List of addons. Possible values are:
  * zillow_3d
  * aerial_addon
  * twilight
  * floor_plans
  * custom_domain

#### Response
```
{
    "id": null,
    "realtor_id": "",
    "created_date": "",
    "status: "",
}
```

#### Sample Request
```
curl --location --request POST 'https://getawalkthrough.com/api/listing' \
--header 'Content-Type: application/json' \
--data-raw '{
   "address":{
      "line_1":"2911 Walnut Street",
      "line_2":"",
      "city":"Denver",
      "state":"CO",
      "zip": 80205
   },
   "sqft":2000,
   "scheduled_time":"2022-05-20T14:00:00",
   "realtor_id": "62337c91bbcada24cfedb807",
   "product_type":"photography_package",
   "addons": [
      "zillow_3d",
      "aerial_addon",
      "twilight",
      "floor_plans",
      "custom_domain"
   ]
}
'
```


## Get listing
*GET **/api/houses/house_id***

#### Parameters
* **house_id** - required string(ObjectId)
  * House id in the walkthrough system. A 12-byte Field Of BSON type

#### Response
```
{
    "id": "",
    "": "", 
    "full_address": null
}
```

## Success Response
* **status** - string
  * success
* **data** - dictionary 
  * Dictionary

## Error Response
* **status** - string
  * error
* **code** - string
  * The error's code if response's status is error. See Error codes below
* **message** - string
  * The error message

## Error Codes
* **invalid_method** - 400
  * This action can only be taken in valid format.
* **unauthorized**
  * Unauthorized - 401
* **outside_service_area** - 400
  * We don't currently serve that zip code. However, we sent a message to our operations team to see if we can make an exception. You can also call support at 303-900-0469.
* **duplicate_entry** - 400
  * It looks like we're already working on that address with you. Go to your dashboard to view the current status or call support at 303-900-0469.
* 