# Cancel an order

### `DELETE` /orders/{pair}/{orderId}

### Parameters

| Header              | Path    | Query | Type   | Required | Description                                                                                                                 | Default | Range | Example    |
| ------------------- | ------- | ----- | ------ | -------- | --------------------------------------------------------------------------------------------------------------------------- | ------- | ----- | ---------- |
| X-BITOPRO-APIKEY    |         |       | string | Yes      | [API Key](../authentication.md#api-key)                                                                                     |         |       |            |
| X-BITOPRO-PAYLOAD   |         |       | string | Yes      | [Payload](../authentication.md#payload)                                                                                     |         |       |            |
| X-BITOPRO-SIGNATURE |         |       | string | Yes      | [Signature](../authentication.md#signature)                                                                                 |         |       |            |
|                     | pair    |       | string | Yes      | The trading pair in format ${BASE}_${QUOTE}, Please follow the [link](https://www.bitopro.com/fees) to check the pair list. |         |       | bito_eth   |
|                     | orderId |       | string | Yes      | The id of the order.                                                                                                        |         |       | 2959906694 |

### Response sample

```js
{
  "action": "BUY",
  "amount": "2.3",
  "orderId": "12234566",
  "price": "1.2",
  "timestamp": 1504262258000
}
```

[Back](../rest.md)