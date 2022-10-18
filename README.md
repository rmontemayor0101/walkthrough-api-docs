# WALKTHROUGH API DOCUMENTATION

## Endpoints
* POST /api/houses
* GET /api/houses/<house_id>

---
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
* **unfinished_sqft** - required int
  * House's unfinished square feet
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
```json
{
  "status": "success",
  "data": {house_object} # See reference below
}
```

#### Sample Request
```bash
curl --location --request POST 'https://getawalkthrough.com/api/houses' \
--header 'Content-Type: application/json' \
--header 'api-key: 74de3164-094d-4456-bda8-33a99f896930' \
--data-raw '{
   "address":{
      "line_1":"2911 Walnut Street",
      "line_2":"",
      "city":"Denver",
      "state":"CO",
      "zip": 80205
   },
   "sqft":2000,
   "unfinished_sqft": 0,
   "scheduled_time": 1666214660,
   "realtor_id": "62337c91bbcada24cfedb807",
   "product_type":"photography_package",
   "addons": [
      "zillow_3d",
      "aerial_addon",
      "twilight",
      "floor_plans",
      "custom_domain"
   ]
}'
```

---

## Get listing
*GET **/api/houses/<house_id>***

#### Parameters
* **house_id** - required string(ObjectId)
  * House id in the walkthrough system. A 12-byte Field Of BSON type

#### Response
```json
{
  "status": "success",
  "data": {house_object} # See reference below
}
```

#### Sample Request
```bash
curl --location --request GET 'https://getawalkthrough.com/api/houses/634c6b5dbbcada0fb65187d1' \
--header 'api-key: 74de3164-094d-4456-bda8-33a99f896930'
```

---

## The House Object
* **id** - string
  * House's id in the walkthrough system.
* **realtor_id** - string
  * Realtor's id in the walkthrough system.
* **status** - string
  * Listing's status. Possible values are:
  * **unscheduled**
    * Needs To Be Scheduled
    * Need To Know How To Get Access
    * Scheduled Awaiting Payment
  * **proofing**
    * Processed
  * **completed**
    * Completed
  * **scheduled**
    * Waiting For Processing
    * Processing
    * Scanned
    * Arrived
    * Ready For Photography
* **date_created** - timestamp
  * Date created. A UTC timestamp

#### Sample Response
```json
{
    "id": "634dd5847fd93f2263fb05d4",
    "realtor_id": "624f658c4f73fbde4cbcc73a",
    "status": "scheduled",
    "date_created": 1666066918,
}
```

---

## Success Response
* **status** - string
  * success
* **data** - dictionary 
  * The house object. See reference above

---

## Error Response
* **status** - string
  * error
* **code** - string
  * The error's code if response's status is error. See Error codes below
* **message** - string
  * The error message

---

## Error Codes
* **invalid_method** - 400
  * This action can only be taken in valid format.
* **unauthorized** - 401
  * Unauthorized 
* **outside_service_area** - 400
  * We don't currently serve that zip code. However, we sent a message to our operations team to see if we can make an exception. You can also call support at 303-900-0469.
* **duplicate_entry** - 400
  * It looks like we're already working on that address with you. Go to your dashboard to view the current status or call support at 303-900-0469.
* **not_found** - 404
  * "Error 404 not found"
* **sqft_below_zero** - 400
  * "We need you to put in a square footage other than 0. This is how we calculate how long your photography shoot will take, and we don't want to have to leave before we are done because of an incorrect square footage.
* **invalid_id** - 400
  * ID is not a valid ObjectId, it must be a 12-byte input or a 24-character hex string
* **realtor_not_found** - 404
  * Error 404 not found