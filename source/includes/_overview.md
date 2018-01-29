Getting started
=========================

## Overview

Calls for ConvertKit API v3 are relative to the url <https://api.convertkit.com/v3/>

API v3 is in active development. Currently it allows you to:

-   Get a list of forms for an account
-   Add subscriber to a form
-   Add subscriber to an email sequence
-   Create a tag
-   Tag a subscriber
-   Remove tags from a subscriber
-   Add, update, and list custom fields

## Planning your integration

Sweet! You're building an integration with ConvertKit. We're thrilled that you want to add-on to our platform. But before you dive in and start slinging code, let's talk about the best way to integrate with ConvertKit.

How you setup your integration depends on what type of provider you are. Let's run through a few examples:

-   **Email Capture.** If your product is mostly for capturing new subscribers then you should integrate with forms in ConvertKit. So in OptinMonster when a subscriber opts-in they are subscriber to a form in ConvertKit.
-   **E-Commerce.** The most important thing for e-commerce customers is the ability to add a tag when a purchase is made. So when SamCart wrote their integration they have the ability to add or remove a subscriber from a tag after a purchase or refund.
-   **Full Integration.** If you want a more full integration with ConvertKit you can add support for forms, courses, and tags.

If you aren't sure how best to structure your integration, just reach out to our team, we'll be happy to help you design it.

## API Basics

### API Key

All API calls require the `api_key` parameter. You can find your API Key in the ConvertKit Account page.

### API Secret

Some API calls require the `api_secret` parameter. All calls that require `api_key` also work with `api_secret`, there's no need to use both. This key grants access to sensitive data and actions on your subscribers, so you should treat it as your password, and do not use it in any client-side code (usually that means JavaScript).

### Responses

When an API call goes well the API will return a 200 or 201 HTTP response, along with a JSON response body.

If there's some error the API will return an HTTP response in the 400 or 500 range, and a response body indicating what the error was, for example:

`{ "error": "Authorization Failed", "message": "API Key not present" }` with a 401 error.

#### Bad data

When you create or update a field, you may receive an HTTP 422 if any fields contain bad data or required fields are missing.

#### Rate limiting

If your request rate exceeds our limits, you will receive an HTTP status of 429 with the message "Too Many Requests".

-   For the subscribe endpoints for forms, sequences, and tags, the rate is no more than 600 requests over any 5 minute period.

#### Internal server errors

If the server is overloaded or you encounter a bug, you will get a 500 error. Try again after a short period, and if you continue to encounter an error, please raise the issue with support.
