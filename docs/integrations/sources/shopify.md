# Shopify
This page contains the setup guide and reference information for the Shopify source connector.

## Setup guide

This connector supports the `OAuth2.0` and `API Password` (for private applications) athentication methods.

### Connect using OAuth2.0 (for Cloud users only)
1. Click `Authenticate your Shopify account` to start the autentication.
2. Click `Install` to install the Airbyte application.
3. Log in to your account, if you are not already logged in.
4. Select the store you want to sync and review the consent.
5. Click on `Install` to finish the Installation.
6. Reveiew the `Shop Name` field for the chosen store for a sync.
7. Set the `Start Date` as the starting point for your data replication. Any data created before this date will not be synced.
8. Click `Test and Save` to finish the source set up.

### Connect using `API PASSWORD` option (for OSS users)
1. Name your source.
2. Enter your Store name. You can find this in your URL when logged in to Shopify or within the Store details section of your Settings.
3. Enter your Admin API access token. To set up the access token, you will need to set up a custom application. See instructions below on creating a custom app.
4. Click Set up source

## Creating a Custom App
Authentication to the Shopify API requies a [custom application](https://help.shopify.com/en/manual/apps/app-types/custom-apps). Follow these instructions to create a custom app and find your Admin API Access Token.

1. Navigate to Settings > App and sales channels > Develop apps > Create an app
2. Name your new app
3. Select **Configure Admin API scopes**
4. Tick all the scopes prefixed with `read_` (e.g. `read_locations`,`read_price_rules`, etc ) and save. See below for the full list of scopes to allow.
5. Click **Install app** to give this app access to your data.
6. Once installed, go to **API Credentials** to copy the **Admin API Access Token**.

You're ready to set up Shopify in Airbyte!

### Scopes Required for Custom App

Add the following scopes to your custom app to ensure Airbyte can sync all available data. To see a list of streams this source supports, see our full [Shopify documentation](https://docs.airbyte.com/integrations/sources/shopify/).

- `read_analytics`
- `read_assigned_fulfillment_orders`
- `read_gdpr_data_request`
- `read_locations`
- `read_price_rules`
- `read_product_listings`
- `read_products`
- `read_reports`
- `read_resource_feedbacks`
- `read_script_tags`
- `read_shipping`
- `read_locales`
- `read_shopify_payments_accounts`
- `read_shopify_payments_bank_accounts`
- `read_shopify_payments_disputes`
- `read_shopify_payments_payouts`
- `read_content`
- `read_themes`
- `read_third_party_fulfillment_orders`
- `read_translations`
- `read_customers`
- `read_discounts`
- `read_draft_orders`
- `read_fulfillments`
- `read_gift_cards`
- `read_inventory`
- `read_legal_policies`
- `read_marketing_events`
- `read_merchant_managed_fulfillment_orders`
- `read_online_store_pages`
- `read_order_edits`
- `read_orders`

## Supported sync modes

The Shopify source supports both Full Refresh and Incremental syncs. You can choose if this connector will copy only the new or updated data, or all rows in the tables and columns you set up for replication, every time a sync is run.

This source can sync data for the [Shopify REST API](https://shopify.dev/api/admin-rest) and the [Shopify GraphQl API](https://shopify.dev/api/admin-graphql).

## Troubleshooting tips

* If you faced with `access` errors while using the `OAuth2.0`, please make sure you've followed this [Shopify Article](https://help.shopify.com/en/partners/dashboard/managing-stores/request-access#request-access) to request the access to the Client's store first, once the access is granted, you should be able to proceed with `OAuth2.0` authentication method.
* Check out common troubleshooting issues for the Shopify source connector on our Airbyte Forum [here](https://github.com/airbytehq/airbyte/discussions).

## Supported Streams

This Source is capable of syncing the following core Streams:

- [Articles](https://shopify.dev/api/admin-rest/2022-01/resources/article)
- [Blogs](https://shopify.dev/api/admin-rest/2022-01/resources/blog)
- [Abandoned Checkouts](https://shopify.dev/api/admin-rest/2022-01/resources/abandoned-checkouts#top)
- [Collects](https://shopify.dev/api/admin-rest/2022-01/resources/collect#top)
- [Collections](https://shopify.dev/api/admin-rest/2022-01/resources/collection)
- [Countries](https://shopify.dev/docs/api/admin-rest/2023-04/resources/country)
- [Custom Collections](https://shopify.dev/api/admin-rest/2022-01/resources/customcollection#top)
- [CustomerAddress](https://shopify.dev/docs/api/admin-rest/2023-04/resources/customer-address)
- [CustomerSavedSearch](https://shopify.dev/docs/api/admin-rest/2023-04/resources/customersavedsearch)
- [Smart Collections](https://shopify.dev/api/admin-rest/2022-01/resources/smartcollection)
- [Customers](https://shopify.dev/api/admin-rest/2022-01/resources/customer#top)
- [Draft Orders](https://shopify.dev/api/admin-rest/2022-01/resources/draftorder#top)
- [Discount Codes](https://shopify.dev/api/admin-rest/2022-01/resources/discountcode#top)
- [Disputes](https://shopify.dev/docs/api/admin-rest/2023-07/resources/dispute)
- [Metafields](https://shopify.dev/api/admin-rest/2022-01/resources/metafield#top)
- [Orders](https://shopify.dev/api/admin-rest/2022-01/resources/order#top)
- [Orders Refunds](https://shopify.dev/api/admin-rest/2022-01/resources/refund#top)
- [Orders Risks](https://shopify.dev/api/admin-rest/2022-01/resources/order-risk#top)
- [Products](https://shopify.dev/api/admin-rest/2022-01/resources/product#top)
- [Products (GraphQL)](https://shopify.dev/api/admin-graphql/2022-10/queries/products)
- [Product Images](https://shopify.dev/api/admin-rest/2022-01/resources/product-image)
- [Product Variants](https://shopify.dev/api/admin-rest/2022-01/resources/product-variant)
- [Transactions](https://shopify.dev/api/admin-rest/2022-01/resources/transaction#top)
- [Tender Transactions](https://shopify.dev/api/admin-rest/2022-01/resources/tendertransaction)
- [Pages](https://shopify.dev/api/admin-rest/2022-01/resources/page#top)
- [Price Rules](https://shopify.dev/api/admin-rest/2022-01/resources/pricerule#top)
- [Locations](https://shopify.dev/api/admin-rest/2022-01/resources/location)
- [InventoryItems](https://shopify.dev/api/admin-rest/2022-01/resources/inventoryItem)
- [InventoryLevels](https://shopify.dev/api/admin-rest/2021-01/resources/inventorylevel)
- [Fulfillment Orders](https://shopify.dev/api/admin-rest/2022-01/resources/fulfillmentorder)
- [Fulfillments](https://shopify.dev/api/admin-rest/2022-01/resources/fulfillment)
- [Shop](https://shopify.dev/api/admin-rest/2022-01/resources/shop)

### Stream sync recommendations

For better experience with `Incremental Refresh` the following is recommended:

- `Order Refunds`, `Order Risks`, `Transactions` should be synced along with `Orders` stream.
- `Discount Codes` should be synced along with `Price Rules` stream.

If child streams are synced alone from the parent stream - the full sync will take place, and the records are filtered out afterwards.

## Capturing Deleted Records
The connector captures deletions for records in the `Articles`, `Blogs`, `CustomCollections`, `Orders`, `Pages`, `PriceRules` and `Products` streams. 

When a record is deleted, the connector outputs a record with the `ID` of that record and the `deleted_at`, `deleted_message`, and `deleted_description` fields filled out. No other fields are filled out for the deleted records. 

Check the following Shopify documentation for more information about the [Retrieving Deleted Records](https://shopify.dev/docs/api/admin-rest/2023-07/resources/event) 


## Data type mapping

| Integration Type | Airbyte Type |
| :--------------- | :----------- |
| `string`         | `string`     |
| `number`         | `number`     |
| `array`          | `array`      |
| `object`         | `object`     |
| `boolean`        | `boolean`    |

## Features

| Feature                   | Supported?\(Yes/No\) |
| :------------------------ | :------------------- |
| Full Refresh Sync         | Yes                  |
| Incremental - Append Sync | Yes                  |
| Namespaces                | No                   |

## Performance considerations

Shopify has some [rate limit restrictions](https://shopify.dev/concepts/about-apis/rate-limits). Typically, there should not be issues with throttling or exceeding the rate limits but in some edge cases, user can receive the warning message as follows:

```text
"Caught retryable error '<some_error> or null' after <some_number> tries. Waiting <some_number> seconds then retrying..."
```

This is expected when the connector hits the 429 - Rate Limit Exceeded HTTP Error. With given error message the sync operation is still goes on, but will require more time to finish.

## Changelog

| Version | Date       | Pull Request                                             | Subject                                                                                                                         |
| :------ | :--------- | :------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------ |
| 1.1.1   | 2023-09-18 | [30560](https://github.com/airbytehq/airbyte/pull/30560) | Performance testing - include socat binary in docker image |
| 1.1.0   | 2023-09-07 | [30246](https://github.com/airbytehq/airbyte/pull/30246) | Added ability to fetch `destroyed` records for `Articles, Blogs, CustomCollections, Orders, Pages, PriceRules, Products` |
| 1.0.0   | 2023-08-11 | [29361](https://github.com/airbytehq/airbyte/pull/29361) | Migrate to the `2023-07` Shopify API Version  |
| 0.6.2   | 2023-08-09 | [29302](https://github.com/airbytehq/airbyte/pull/29302) | Handle the `Internal Server Error` when entity could be fetched                 |
| 0.6.1   | 2023-08-08 | [28291](https://github.com/airbytehq/airbyte/pull/28291) | Allow `shop` field to accept `*.myshopify.com` shop names, updated `OAuth Spec`                  |
| 0.6.0   | 2023-08-02 | [28770](https://github.com/airbytehq/airbyte/pull/28770) | Added `Disputes` stream  |
| 0.5.1   | 2023-07-13 | [28700](https://github.com/airbytehq/airbyte/pull/28700) | Improved `error messages` with more user-friendly description, refactored code  |
| 0.5.0   | 2023-06-13 | [27732](https://github.com/airbytehq/airbyte/pull/27732) | License Update: Elv2                                                                                                            |
| 0.4.0   | 2023-06-13 | [27083](https://github.com/airbytehq/airbyte/pull/27083) | Added `CustomerSavedSearch`, `CustomerAddress` and `Countries` streams                                                          |
| 0.3.4   | 2023-05-10 | [25961](https://github.com/airbytehq/airbyte/pull/25961) | Added validation for `shop` in input configuration (accepts non-url-like inputs)                                                |
| 0.3.3   | 2023-04-12 | [25110](https://github.com/airbytehq/airbyte/pull/25110) | Fixed issue when `cursor_field` is `"None"`, added missing properties to stream schemas, fixed `access_scopes` validation error |
| 0.3.2   | 2023-02-27 | [23473](https://github.com/airbytehq/airbyte/pull/23473) | Fixed OOM / Memory leak issue for Airbyte Cloud                                                                                 |
| 0.3.1   | 2023-01-16 | [21461](https://github.com/airbytehq/airbyte/pull/21461) | Added `discount_applications` to `orders` stream                                                                                |
| 0.3.0   | 2022-11-16 | [19492](https://github.com/airbytehq/airbyte/pull/19492) | Added support for graphql and add a graphql products stream                                                                     |
| 0.2.0   | 2022-10-21 | [18298](https://github.com/airbytehq/airbyte/pull/18298) | Updated API version to the `2022-10`, make stream schemas backward cpmpatible                                                   |
| 0.1.39  | 2022-10-13 | [17962](https://github.com/airbytehq/airbyte/pull/17962) | Added metafield streams; support for nested list streams                                                                        |
| 0.1.38  | 2022-10-10 | [17777](https://github.com/airbytehq/airbyte/pull/17777) | Fixed `404` for configured streams, fix missing `cursor` error for old records                                                  |
| 0.1.37  | 2022-04-30 | [12500](https://github.com/airbytehq/airbyte/pull/12500) | Improve input configuration copy                                                                                                |
| 0.1.36  | 2022-03-22 | [9850](https://github.com/airbytehq/airbyte/pull/9850)   | Added `BalanceTransactions` stream                                                                                              |
| 0.1.35  | 2022-03-07 | [10915](https://github.com/airbytehq/airbyte/pull/10915) | Fixed a bug which caused `full-refresh` syncs of child REST entities configured for `incremental`                               |
| 0.1.34  | 2022-03-02 | [10794](https://github.com/airbytehq/airbyte/pull/10794) | Minor specification re-order, fixed links in documentation                                                                      |
| 0.1.33  | 2022-02-17 | [10419](https://github.com/airbytehq/airbyte/pull/10419) | Fixed wrong field type for tax_exemptions for `Abandoned_checkouts` stream                                                      |
| 0.1.32  | 2022-02-18 | [10449](https://github.com/airbytehq/airbyte/pull/10449) | Added `tender_transactions` stream                                                                                              |
| 0.1.31  | 2022-02-08 | [10175](https://github.com/airbytehq/airbyte/pull/10175) | Fixed compatibility issues for legacy user config                                                                               |
| 0.1.30  | 2022-01-24 | [9648](https://github.com/airbytehq/airbyte/pull/9648)   | Added permission validation before sync                                                                                         |
| 0.1.29  | 2022-01-20 | [9049](https://github.com/airbytehq/airbyte/pull/9248)   | Added `shop_url` to the record for all streams                                                                                  |
| 0.1.28  | 2022-01-19 | [9591](https://github.com/airbytehq/airbyte/pull/9591)   | Implemented `OAuth2.0` authentication method for Airbyte Cloud                                                                  |
| 0.1.27  | 2021-12-22 | [9049](https://github.com/airbytehq/airbyte/pull/9049)   | Updated connector fields title/description                                                                                      |
| 0.1.26  | 2021-12-14 | [8597](https://github.com/airbytehq/airbyte/pull/8597)   | Fixed `mismatched number of tables` for base-normalization, increased performance of `order_refunds` stream                     |
| 0.1.25  | 2021-12-02 | [8297](https://github.com/airbytehq/airbyte/pull/8297)   | Added Shop stream                                                                                                               |
| 0.1.24  | 2021-11-30 | [7783](https://github.com/airbytehq/airbyte/pull/7783)   | Reviewed and corrected schemas for all streams                                                                                  |
| 0.1.23  | 2021-11-15 | [7973](https://github.com/airbytehq/airbyte/pull/7973)   | Added `InventoryItems`                                                                                                          |
| 0.1.22  | 2021-10-18 | [7101](https://github.com/airbytehq/airbyte/pull/7107)   | Added FulfillmentOrders, Fulfillments streams                                                                                   |
| 0.1.21  | 2021-10-14 | [7382](https://github.com/airbytehq/airbyte/pull/7382)   | Fixed `InventoryLevels` primary key                                                                                             |
| 0.1.20  | 2021-10-14 | [7063](https://github.com/airbytehq/airbyte/pull/7063)   | Added `Location` and `InventoryLevels` as streams                                                                               |
| 0.1.19  | 2021-10-11 | [6951](https://github.com/airbytehq/airbyte/pull/6951)   | Added support of `OAuth 2.0` authorisation option                                                                               |
| 0.1.18  | 2021-09-21 | [6056](https://github.com/airbytehq/airbyte/pull/6056)   | Added `pre_tax_price` to the `orders/line_items` schema                                                                         |
| 0.1.17  | 2021-09-17 | [5244](https://github.com/airbytehq/airbyte/pull/5244)   | Created data type enforcer for converting prices into numbers                                                                   |
| 0.1.16  | 2021-09-09 | [5965](https://github.com/airbytehq/airbyte/pull/5945)   | Fixed the connector's performance for `Incremental refresh`                                                                     |
| 0.1.15  | 2021-09-02 | [5853](https://github.com/airbytehq/airbyte/pull/5853)   | Fixed `amount` type in `order_refund` schema                                                                                    |
| 0.1.14  | 2021-09-02 | [5801](https://github.com/airbytehq/airbyte/pull/5801)   | Fixed `line_items/discount allocations` & `duties` parts of `orders` schema                                                     |
| 0.1.13  | 2021-08-17 | [5470](https://github.com/airbytehq/airbyte/pull/5470)   | Fixed rate limits throttling                                                                                                    |
| 0.1.12  | 2021-08-09 | [5276](https://github.com/airbytehq/airbyte/pull/5276)   | Added status property to product schema                                                                                         |
| 0.1.11  | 2021-07-23 | [4943](https://github.com/airbytehq/airbyte/pull/4943)   | Fixed products schema up to API 2021-07                                                                                         |
| 0.1.10  | 2021-07-19 | [4830](https://github.com/airbytehq/airbyte/pull/4830)   | Fixed for streams json schemas, upgrade to API version 2021-07                                                                  |
| 0.1.9   | 2021-07-04 | [4472](https://github.com/airbytehq/airbyte/pull/4472)   | Incremental sync is now using updated_at instead of since_id by default                                                         |
| 0.1.8   | 2021-06-29 | [4121](https://github.com/airbytehq/airbyte/pull/4121)   | Added draft orders stream                                                                                                       |
| 0.1.7   | 2021-06-26 | [4290](https://github.com/airbytehq/airbyte/pull/4290)   | Fixed the bug when limiting output records to 1 caused infinity loop                                                            |
| 0.1.6   | 2021-06-24 | [4009](https://github.com/airbytehq/airbyte/pull/4009)   | Added pages, price rules and discount codes streams                                                                             |
| 0.1.5   | 2021-06-10 | [3973](https://github.com/airbytehq/airbyte/pull/3973)   | Added `AIRBYTE_ENTRYPOINT` for Kubernetes support                                                                               |
| 0.1.4   | 2021-06-09 | [3926](https://github.com/airbytehq/airbyte/pull/3926)   | New attributes to Orders schema                                                                                                 |
| 0.1.3   | 2021-06-08 | [3787](https://github.com/airbytehq/airbyte/pull/3787)   | Added Native Shopify Source Connector                                                                                           |
