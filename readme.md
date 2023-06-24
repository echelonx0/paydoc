<!-- @format -->

# Payment API Documentation

## Introduction

The Payment API allows our partners to issue payment instructions for us to pay 3rd parties. It includes two main endpoints:

- `POST /payments`
- `GET /payments/:id`

## POST /payments

Allows creating a new payment.

### Request

#### Body Parameters

- `paymentType` - A number 0 or 1 which refers to payout or receiving. (Required)
- `clientBank` - A string representing the client's bank. (Required)
- `clientAccountType` - A string representing the client's account type. (Required)
- `clientName` - A string representing the client's name. (Required)
- `clientAccountNumber` - A string or number representing the client's account number. (Required)
- `transactionAmount` - A double representing the transaction amount. (Required)
- `transactionCurrency` - A string representing the transaction currency. (Required)
- `transactionID` - A string representing the transaction ID. (Required)
- `transactionCountry` - A string representing the transaction country. (Required)
- `validationCode` - A string representing the validation code. (Required)

```json
{
  "paymentType": 0,
  "clientBank": "Bank of Test",
  "clientAccountType": "Savings",
  "clientName": "John Doe",
  "clientAccountNumber": "12345678",
  "transactionAmount": 1500.0,
  "transactionCurrency": "USD",
  "transactionID": "T1234",
  "transactionCountry": "US",
  "validationCode": "123456"
}
```

### Response

#### Success (HTTP Status Code: 201)

- `transactionHash` - A string representing the hash of all the transaction parameters plus the timestamp of when the transaction was received.

```json
{
  "message": "Created transaction successfully",
  "createdTransaction": {
    "_id": "60f7f8734d2b5e210c3c28ea",
    "paymentType": 0,
    "clientBank": "Bank of Gotham City",
    "clientAccountType": "Savings",
    "clientName": "Bruce Wayne",
    "clientAccountNumber": "12345678",
    "transactionAmount": 1500,
    "transactionCurrency": "USD",
    "transactionID": "T1234",
    "transactionCountry": "US",
    "validationCode": "123456",
    "transactionHash": "a8d3a48eafe1b2b76f29f74cf1f8552adfe3b7b3a05a1e0e378e9fb69d7e3788",
    "request": {
      "type": "GET",
      "url": "https://citadel-tools.uc.r.appspot.com/payments/60f7f8734d2b5e210c3c28ea"
    }
  }
}
```

#### Error (HTTP Status Code: 500)

- `error` - A string describing the error.

```json
{
  "error": "Error message"
}
```

## GET /payments/:id

Allows you to get the details of any specific payment.

### Request

#### Path Parameters

- `id` - The ID of the transaction.

`GET /payments/60f7f8734d2b5e210c3c28ea`

### Response

#### Success (HTTP Status Code: 200)

- `transactionHash` - The transaction hash which was saved earlier when we received the request.
- `validationCode` - The validation code.
- `paymentStatus` - The payment status

which is a number like 200, etc.

```json
{
  "transaction": {
    "_id": "60f7f8734d2b5e210c3c28ea",
    "paymentType": 0,
    "clientBank": "Bank of Gotham City",
    "clientAccountType": "Savings",
    "clientName": "John Wayne",
    "clientAccountNumber": "12345678",
    "transactionAmount": 1500,
    "transactionCurrency": "USD",
    "transactionID": "T1234",
    "transactionCountry": "US",
    "validationCode": "123456",
    "transactionHash": "a8d3a48eafe1b2b76f29f74cf1f8552adfe3b7b3a05a1e0e378e9fb69d7e3788",
    "paymentStatus": 200,
    "request": {
      "type": "GET",
      "url": "https://citadel-tools.uc.r.appspot.com/payments/"
    }
  }
}
```

---

## Status Definitions

Our API uses numeric status codes to indicate the state of each transaction. Here are what the status codes represent:

- `0`: Payment Pending - The payment instruction has been received and is currently being processed.
- `1`: Paid - The payment has been successfully processed and the funds have been transferred.
- `2`: Error: Payment Details Incorrect - There was an error processing the payment due to incorrect payment details. Please verify the details and try again.
- `3`: Error: Bank Services Unavailable - There was an error processing the payment because the bank's services are currently unavailable. Please try again later.

These status codes can be found in the `status` field of the response when you make a GET request to `https://citadel-tools.uc.r.appspot.com/payments/{transaction_id}`.

---

Please let me know if you need any more changes or additions to your documentation!

#### Error (HTTP Status Code: 404)

- `message` - A string with the error message.

```json
{
  "message": "No valid entry found for provided ID"
}
```

#### Error (HTTP Status Code: 500)

- `error` - A string describing the error.

```json
{
  "error": "Error message"
}
```
