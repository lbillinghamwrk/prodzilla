# Prodzilla 🦖

Prodzilla is a modern synthetic monitoring tool built in Rust. It's focused on surfacing whether existing behaviour in production is as expected in a human-readable format, so that even customers or stakeholders can contribute to system verification. 

A SaaS option will be available in the future. More at [prodzilla.io](https://prodzilla.io/).

It's currently in active development.

## Usage

The application parses the `prodzilla.yml` file to generate a list of probes executed on a given schedule:

```yml
probes:
  - name: Example A
    url: http://facebook.com
    http_method: GET
    expect_back:
      status_code: "200"
    schedule:
      initial_delay: 5
      interval: 10
  - name: Example B
    url: http://google.com
    http_method: GET
    expect_back:
      status_code: "200"
    schedule:
      initial_delay: 2
      interval: 6
```

## Feature Roadmap

:white_check_mark: = Ready
:bricks: = In development

- Protocol Support
    - HTTP / HTTPS Calls
        - GET :white_check_mark:
        - POST :bricks:
        - PUT
        - PATCH
    - Grpc
- Request Construction
    - Add headers :bricks:
    - Add body :bricks:
- Response Validation
    - Status code
    - Response body
    - Specific fields
- Authentication
    - Bearer Tokens
    - Requests
- Yaml Objects - Reusable parameters
    - Request bodies
    - Authenticated users
    - Validation
- Result storage
    - NativeDB?
- UI / Output
    - JSON output of results for all probes
    - UI output of results for all probes
- Forwarding alerts
    - Webhooks
    - Email
    - Splunk / OpsGenie / PagerDuty / slack integrations?
- Complex Tests
    - Retries
    - Chained queries
    - Parameters in queries
    - Parametrized tests
- CI / CD Integration
    - Standalone easy-to-install image
    - Github Actions integration to trigger tests / use as smoke tests

## Future prodzilla.yml definition 

I'd love to get to a point where the yml file represents a view of the intended behaviour of the services that any human / stakeholder can read and understand.

A full view of this is available in `prodzilla-future.yml`.

```yml
stories:
  - name: Create cardholder card
    steps:
      - name: Create cardholder
        as: CardholderUser
        url: https://api.airwallex.com/api/v1/issuing/cardholders/create
        http_method: POST
        with: CreateCardholderRequest
        expect_back: ValidCreateCardholderResponse

      - name: Create card
        as: CardholderUser
        url: https://api.airwallex.com/api/v1/issuing/cards/create
        http_method: POST
        with: CreateCardRequest
        expect_back: ValidCreateCardResponse

      - name: Get card in Admin panel
        as: CardholderUser
        url: https://api.airwallex.com/api/v1/issuing/cards/create
        http_method: POST
        with: CreateCardRequest
        expect_back: ValidCreateCardResponse

    schedule: EveryMinute10sDelay
```