## Instant Sell Recent Items

This endpoint can be used to instant-sell recently (60 min) unboxed items on OPSkins. Items are automatically transferred to OPSkins and then sold via the endpoint [ISales/InstantSellItems/v1](https://docs.opskins.com/public/en.html#ISales_InstantSellItems_v1). Note that partial success is possible with this endpoint. It's also possible that we will send a `status` of `1` but the OPSkins endpoint will fail completely, as shown in the Output Examples below.

#### HTTP Request

`POST https://api-trade.wax.io/IItem/InstantSellRecentItems/v1/`

#### Authentication

API key required.

#### OAuth Scopes
`instant_sell_recent_items`
- If using OAuth, OPSkins wallet balance information will not be shown unless `balance` scope is available.

#### Allowed Apps
- All apps & items allowed. Note that the OPSkins endpoint may still reject some apps & items.

#### Input Descriptions

Parameter | Type | Required   | Description
--------- | -----| :--------: | -----------
item_id| int-csv | + | List of Item IDs, separated with commas. Maximum `100`.
instant_sell_type | int | | `1` for OPSkins Credits, `2` for USD (default)

#### Output Descriptions
Parameter | Type | Description
--------- | ---- | -----------
valid_item_ids | array-int | Item IDs considered valid
unknown_item_ids | array-int | Item IDs that were not found in the database or do not belong to you
not_recent_item_ids | array-int | Item IDs created more than 60 minutes ago, which are not eligible
ineligible_item_ids | array-int | Item IDs that are currently not eligible for trade or transfer
not_allowed_item_ids | array-int | Deprecated (all apps & items allowed). Item IDs that are not allowed for this endpoint. See [Allowed Apps](/IItem/InstantSellRecentItems.md#allowed-apps) above.
isales_instantsellitems_v1 | mixed | Full [ISales/InstantSellItems/v1](https://docs.opskins.com/public/en.html#ISales_InstantSellItems_v1) response from OPSkins API

#### Output Examples
Success (partial):

```json
{
    "status": 1,
    "time": 1542928287,
    "response": {
        "valid_item_ids": [
            391
        ],
        "unknown_item_ids": [ 
            291, 
	    292
        ],
        "not_recent_item_ids": [],
        "ineligible_item_ids": [],
        "not_allowed_item_ids": [],
        "isales_instantsellitems_v1": {
            "status": 1,
            "time": 1542928287,
            "balance": 500027520,
            "credits": 245,
            "cryptoBalances": {
                "ETH": "0.000000000000000000",
                "WAX": "0.000000000000000000"
            },
            "response": {
                "items": [
                    {
                        "saleid": 309421537,
                        "new_itemid": 309421538,
                        "item_id": 391,
                        "name": "Huntsman Knife | Cyber Sport (Battle-Scarred)"
                    }
                ],
                "items_count": 1,
                "total_value": {
                    "usd": 6601,
                    "credits": 0
                }
            }
        }
    }
}
```

None of the provided items exist or belong to you:
```json
{
    "status": 312,
    "time": 1542910778,
    "message": "None of the items provided exist or belong to you: 159, 160"
}
```

None of the items provided are valid/eligible:
```json
{
    "status": 312,
    "time": 1542910641,
    "message": "None of the items provided are valid/eligible",
    "response": {
        "valid_item_ids": [],
        "unknown_item_ids": [
            159,
            160
        ],
        "not_recent_item_ids": [
            180
        ],
        "ineligible_item_ids": [],
        "not_allowed_item_ids": []
    }
}
```


OPSkins API Error:
```json
{
    "status": 1,
    "time": 1542852394,
    "response": {
        "isales_instantsellitems_v1": {
            "status": 2000,
            "time": 1542852394,
            "message": "Something went wrong."
        }
    }
}
```

Error while transferring items to OPSkins:
```json
{
    "status": 202,
    "time": 1542864143,
    "message": "Error during transfer of items to OPSkins. It's possible the items were transferred successfully.",
    "response": {
        "valid_item_ids": [
            174
        ],
        "unknown_item_ids": [
            159,
            160
        ],
        "not_recent_item_ids": [],
        "ineligible_item_ids": [],
        "not_allowed_item_ids": []
    }
}
```
