# Webhooks for Facebook Leads Ads

## Overview: Create integrations between Facebook Lead Ads and Webhooks

This document provides an in-depth guide on setting up and managing webhooks for lead notifications. Webhooks are a powerful feature that allows you to receive real-time updates about interactions with your Page's Lead ads. By integrating webhooks, you can streamline your lead management process and ensure timely follow-ups.

## How to create webhook in AWS Environment

1. We need to create a GET and POST webhook in AWS API Gateway. for eg. GET /facebookwebhook, POST /facebookwebhook
2. GetMethod resource
    Properties should have Authorization: NONE
    HttpMethod: GET
    Integration:
        Type: AWS_PROXY
        Uri: should point to your lambda
        other proerties
3. You lambda should receive event with queryStringParameters.

   queryStringParameters = {
        "hub.challenge" : "SOME RANDOM NUMBER SEND BY FACEBOOK",
        "hub.mode": "subscribe"
        "hub.verify_token": "YOUR VERIFICATION STRING"
        }
4. Here verify_token is sent while subscribing to Ad Account object.
   refer step below ##Create an facebook developer account >> 4th point
5. Return hub.challenge value from your Lambda
6. This will allow facebook to verify your endpoint
7. Similary, create PostMethod resource of API Gateway
8. If you do step ## Testing your Facebook Leads to webhook
    lambda will receive event having leads high level details.
    we have to now call facebook api to get the details for that lead.
    REF: https://developers.facebook.com/docs/marketing-api/guides/lead-ads/retrieving
9. Once you receive details of leads, you may save data in your database.

## Create an facebook developer account

https://developers.facebook.com
Create an app with Type=Business

Summary: Buiness app creates or manage business assets like Pages, Events, Groups, Ads, Messenger, WhatsApp, and Instagram Graph API using the available business permissions, features, and products.

1. Go to App Settings >> Advanced 
    a> Go to App Page section (facebook page must be same as facebook name)
2. Ref: https://developers.facebook.com/docs/development/create-an-app/other-app-types
3. Put this facebook app in live mode.
4. Go to https://developers.facebook.com/apps/<APP_ID>/dashboard/#addProduct
    a> Go to Add products to your app section
    b> Choose webhooks, click on setup
    c> Choose Ad account from dropdown >> click on subscribe to this object
    c> Enter Callback URL (It should be a GET endpoint)
    d> Enter Verify token = VERIFY_TOKEN

## Subscribe to leadgen: Page Subscribed Apps

POST /v20.0/{page-id}/subscribed_apps HTTP/1.1
Host: graph.facebook.com

You can use Graph API Explorer to subscribe to app
https://developers.facebook.com/tools/explorer/?method=GET&path=%7Bpage-id%7D%2Fsubscribed_apps&version=v20.0
add query string and it's value subscribed_fields=leadgen

Return Type
{
  success: true
}

REF: https://developers.facebook.com/docs/graph-api/reference/page/subscribed_apps/


## Create an facebook adsmanager account

https://adsmanager.facebook.com/
1. Enter your Business details
2. Enter Payment Method (you may skip till you want to put a live campaign)
3. Campaign setup
    a> select Leads type
    b> Choose Manual leads campaign
    c> click on create campaign
4. Create your campaign

REF: https://www.facebook.com/business/help/791294492679966


## Testing your Facebook Leads to webhook

https://developers.facebook.com/tools/lead-ads-testing/
1. Select your page
2. Select your form
3. click on preview form to fill the leads form & submit to generate lead & trigger your POST webhook endpoint

## Author
E-mail: dev.mithilesh2mik@gmail.com