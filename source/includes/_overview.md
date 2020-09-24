Getting started
=========================

## Overview

Calls for ConvertKit API v3 are relative to the url <https://api.convertkit.com/v3/>

API v3 is in active development.

## Planning your integration

Sweet! You're building an integration with ConvertKit. We're thrilled that you want to add-on to our platform. But before you dive in and start slinging code, let's talk about the best way to integrate with ConvertKit.

How you setup your integration depends on what type of provider you are. Let's run through a few examples:

-   **Email Capture.** If your product is mostly for capturing new subscribers then you should integrate with forms in ConvertKit. So in OptinMonster when a subscriber opts-in they are subscriber to a form in ConvertKit.
-   **E-Commerce.** The most important thing for e-commerce customers is the ability to add a tag when a purchase is made. So when SamCart wrote their integration they have the ability to add or remove a subscriber from a tag after a purchase or refund.
-   **Full Integration.** If you want a more full integration with ConvertKit you can add support for forms, courses, and tags.

If you aren't sure how best to structure your integration, just reach out to our team. We'll be happy to help you design it.

## Building a larger integration?

If you're working with a company that's building a large integration with ConvertKit, please [fill out this form](https://docs.google.com/forms/d/e/1FAIpQLSdirDoKSD6q018u9PPGLoX5xstF3_LykXzbacuIi9N57warEg/viewform) to get an integration key. We'll work with you to ensure your integration works amazingly for our mutual customers.

## API Basics

### API Key

All API calls require the `api_key` parameter. You can find your API Key in the ConvertKit Account page.

### API Secret

Some API calls require the `api_secret` parameter. All calls that require `api_key` also work with `api_secret`, there's no need to use both. This key grants access to sensitive data and actions on your subscribers. You should treat it as your password and do not use it in any client-side code (usually that means JavaScript).

### Responses

When an API call succeeds, the API will return a 200 or 201 HTTP response and a JSON response body unless otherwise noted.

If there's some error, the API will return an HTTP response in the 400 or 500 range and a response body indicating what the error was. For example:

`{ "error": "Authorization Failed", "message": "API Key not present" }` with a 401 error.

#### Bad data

When you create or update a field, you may receive an HTTP 422 if any fields contain bad data or required fields are missing.

#### Rate limiting

Our rate limit is no more than 120 requests over a rolling 60 second period, for a given api key.

If your request rate exceeds our limits, you will receive a 429 response, which your code should gracefully handle.  We recommend spacing out your requests and performing an [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff) to keep within the limit.


#### Internal server errors

If the server is overloaded or you encounter a bug, you will get a 500 error. Try again after a short period, and if you continue to encounter an error, please raise the issue with support.
