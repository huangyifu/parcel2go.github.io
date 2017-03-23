---
menu:
  main:
    identifier: international-orders
    weight: 50
title: International Orders
---
International shipments require additional customs details which are provided below :

* ExportReason - The reason for exporting the item, acceptable values are:
  * Sale 
  * Gift
  * Sample
  * Repair
  * Documents
  * IntraCompanyTransfer
  * TemporaryExport

* VatStatus - This is the VAT status of the sender, one of :
  * Individual
  * NotRegistered
  * Registered

* VatNumber - The senders VAT number if VAT status is Registered

* NifNumber - Tax identification number for spanish companies
* EoriNumber - [Economic Operator Registration and Identification Number](https://www.gov.uk/eori) - For registered companies.

* OriginCountry - This is the ISO3 country code which the item was manufactured in.



### Example of a failed internation order

This is an example of a response when customs is required but not specified.

``` json
{
  "Errors": [
    {
      "Type": "Customs",
      "Error": "Please supply the reason for exporting your parcel.",
      "ItemId": "c1361416-53ca-4d67-974a-60f5335cdaf1"
    },
    {
      "Type": "Customs",
      "Error": "Please supply your VAT status.",
      "ItemId": "c1361416-53ca-4d67-974a-60f5335cdaf1"
    },
    {
      "Type": "Customs",
      "Error": "Please supply details of your parcel's country of manufacture.",
      "ItemId": "c1361416-53ca-4d67-974a-60f5335cdaf1"
    },
    {
      "Type": "Customs",
      "Error": "Please supply details of your international parcel's contents.",
      "ItemId": "c1361416-53ca-4d67-974a-60f5335cdaf1"
    },
    {
      "Type": "Service",
      "Error": "A price could not be found for these specifications.",
      "ItemId": "c1361416-53ca-4d67-974a-60f5335cdaf1"
    }
  ]
}

```

### Example of an order specifying customs data

This is an internation order request which properly specifies customs information

``` json

{
  "Items": [
    {
      "Id": "c1361416-53ca-4d67-974a-60f5335cdaf1",
      "CollectionDate": "2017-02-09T10:09:11.9730419+00:00",
      "Parcels": [
        {
          "Id": "c1361416-53ca-4d67-974a-60f5335cdaf1",
          "Height": 10,
          "Length": 10,
          "EstimatedValue": 10,
          "Weight": 1,
          "Width": 10,
          "DeliveryAddress": {
            "ContactName": "Gary Wilson",
            "Email": "g.wilson@parcel2go.com",
            "Phone": "+4477777777777",
            "Organisation": "",
            "Property": "1",
            "Street": "S Brooklyn View Ln",
            "Locality": "",
            "Town": "South Jordan",
            "County": "UTah",
            "Postcode": "84095",
            "CountryIsoCode": "USA"
          },
          "ContentsSummary": "Slippers",
          // Additional details of contents provided
          "Contents": [
            {
              "Description": "Slippers",
              "Quantity": "1",
              "EstimatedValue": "10"
            }
          ]
        }
      ],
      "Service": "parcelforce-global-priority",
      "CollectionAddress": {
        "ContactName": "Test",
        "Email": "collection@example.com",
        "Phone": "+4477777777777",
        "Property": "11",
        "Street": "Saville St",
        "Town": "Malton",
        "County": "North Yorkshire",
        "Postcode": "YO17 7LL",
        "CountryIsoCode": "GBR"
      },
      // additional details for customs below
      "OriginCountry": "GBR",
      "ExportReason": "Sale",
      "VatStatus": "Individual"
    }
  ],
  "CustomerDetails": {
    "Email": "jj@parcel2go.com",
    "Forename": "Joe",
    "Surname": "Joggs"
  }
}

```