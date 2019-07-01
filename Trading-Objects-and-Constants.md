:chart: All content on this page applies to [Trading Terminal](Trading-Terminal) only.

**NOTE:** If you use TypeScript - you can import these constants/interfaces/types from the `broker-api.d.ts` file.

## Broker Configuration

### configFlags: object

This is an object that should be passed in the constructor of the Trading Terminal to [brokerConfig](Widget-Constructor#brokerConfig). Each field should have a boolean value (`true`/`false`):

* `supportReversePosition`

    *Default:* `false`

    Broker supports reversing of a position.
    If it is not supported by broker, Chart will have the reverse button, but it will place a reversing order.

* `supportClosePosition`

    *Default:* `false`

    Broker supports closing of a position.
    If it is not supported by broker, Chart will have the close button, but it will place a closing order.

* `supportReducePosition`

    *Default:* `false`

    Broker supports changing of a position without orders.

* `supportPLUpdate`

    *Default:* `false`

    Broker provides PL for a position. If the broker calculates profit/loss by itself it should call [plUpdate](Trading-Host#plupdatepositionid-pl) as soon as PL is changed.
    Otherwise Chart will calculate PL as a difference between the current trade and an average price of the position.

* `supportOrderBrackets`

    *Default:* `false`

    Broker supports brackets (take profit and stop loss) for orders.
    If this flag is set to `true` the Chart will display additional fields in the order ticket and Modify button on a chart and in the Account Manager.

* `supportPositionBrackets`

    *Default:* `false`

    Broker supports brackets (take profit and stop loss orders) for positions.
    If this flag is set to `true` the Chart will display an Edit button for positions and add `Edit position...` to the context menu of a position.

* `supportTradeBrackets`

    *Default:* `false`

    Broker supports brackets for trades (take profit and stop loss orders).
    If this flag is set to `true` the Chart will display an Edit button for trades (individual positions) and add `Edit position...` to the context menu of a trade.

* `supportTrades`

    *Default:* `false`

    Broker supports individual positions (trades).
    If it is set to `true`, there will be two tabs in the Account Manager - Individual Positions and Net Positions.

* `requiresFIFOCloseTrades`

    *Default:* `false`

    Trading account requires closing of trades in FIFO order.

* `supportCloseTrade`

    *Default:* `false`

    Individual positions (trades) can be closed.

* `supportMultiposition`

    *Default:* `false`

    Supporting multiposition prevents creating the default implementation for a reversing position.

* `showQuantityInsteadOfAmount`

    *Default:* `false`

    This flag can be used to change "Amount" to "Quantity" in the order dialog

* `supportLevel2Data`

    *Default:* `false`

    Level2 data is used for DOM widget. `subscribeDepth` and `unsubscribeDepth` should be implemented.

* `supportMarketOrders`

    *Default:* `true`

    This flag adds market orders type to the order dialog.

* `supportLimitOrders`

    *Default:* `true`

    This flag adds limit orders type to the order dialog.

* `supportStopOrders`

    *Default:* `true`

    This flag adds stop orders type to the order dialog.

* `supportStopLimitOrders`

    *Default:* `false`

    This flag adds stop-limit orders type to the order dialog.

* `supportMarketBrackets`

    *Default:* `true`

    Using this flag you can disable brackets for market orders. It is enabled by default.

* `supportModifyDuration`

    *Default:* `false`

    Using this flag you can enable modification of the duration of the existing order. It is disabled by default.

* `supportModifyOrder`

    *Default:* `true`

    Using this flag you can disable modification of the existing order. It is enabled by default.

### durations: array of objects

List of expiration options of orders. It is optional. Do not set it if you don't want the durations to be displayed in the order ticket.
The objects have two keys: `{ name, value }`.

Example:

```javascript
durations: [{ name: 'DAY', value: 'DAY' }, { name: 'GTC', value: 'GTC' }]
```

### customNotificationFields: array of strings

Optional field. You can use it if you have custom fields in orders or positions that should be taken into account when showing notifications.

For example, if you have field `additionalType` in orders and you want the chart to show a notification when it is changed, you should set:

```javascript
customNotificationFields: ['additionalType']
```

## Order

Describes a single order.

* `id` : String
* `symbol` : String
* `brokerSymbol` : String. Can be empty if broker symbol is the same as TV symbol.
* `type` : [OrderType](#ordertype)
* `side` : [Side](#side)
* `qty` : Double
* `status` : [OrderStatus](#orderstatus)
* `limitPrice` : double
* `stopPrice` : double
* `avgPrice` : double
* `filledQty` : double
* `parentId` : String. If order is a bracket parentOrderId should contain base order/position id.
* `parentType`: [ParentType](#parenttype)
* `duration`: [OrderDuration](#orderduration)

## Position

Describes a single position.

* `id`: String. Usually id should be equal to brokerSymbol
* `symbol` : String
* `brokerSymbol` : String. Can be empty if broker symbol is the same as TV symbol.
* `qty` : positive number
* `side`: [Side](#side)
* `avgPrice` : number

## Trade

Describes a single trade (individual position).

* `id`: String. Usually id should be equal to brokerSymbol
* `symbol` : String
* `date`: number (UNIX timestamp in milliseconds)
* `brokerSymbol` : String. Can be empty if broker symbol is the same as TV symbol.
* `qty` : Double positive
* `side`: [Side](#side)
* `price` : number

## Execution

Describes a single execution. Execution is a mark on a chart that displays trade information.

* `symbol` : String
* `brokerSymbol` : String. Can be empty if broker symbol is the same as TV symbol.
* `price` : number
* `time`: Date
* `side` : [Side](#side)
* `qty` : number

## ActionMetainfo

Describes a single action to put it into a dropdown or a context menu. It is a structure.

* `text` : String
* `checkable` : Boolean. Set it to true if you need a checkbox.
* `checked` : Boolean. Value of the checkbox.
* `enabled`: Boolean
* `action`: function. Action is executed when user clicks the item. It has 1 argument - value of the checkbox if exists.

## OrderType

Constants describing an order status.

```javascript
OrderType.Limit = 1
OrderType.Market = 2
OrderType.Stop = 3
OrderType.StopLimit = 4
```

## Side

Constants describing an order/execution side.

```javascript
Side.Buy = 1
Side.Sell = -1
```

## ParentType

Constants describing a bracket order.

```javascript
ParentType.Order = 1
ParentType.Position = 2
```

## OrderStatus

Constants describing an order status.

```javascript
OrderStatus.Canceled = 1 // order is canceled
OrderStatus.Filled = 2 // order is fully executed
OrderStatus.Inactive = 3 // bracket order is created but waiting for a base order to be filled
OrderStatus.Placing = 4 // order is not created on a broker side yet
OrderStatus.Rejected = 5 // order is rejected for some reason
OrderStatus.Working = 6 // order is created but not executed yet
```

## OrderDuration

Duration or expiration of an order.

* `type`: string identifier from the list that you pass to [durations](#orderduration)
* `datetime`: number

## DOMEObject

Object that describes a single DOM response.

* `snapshot`: Boolean

    Positive value means that previous data should be cleaned

* `asks`: array of DOMELevel sorted by price in ascending order
* `bids`: array of DOMELevel sorted by price in ascending order

## DOMELevel

Single DOM price level object.

* `price`: double
* `volume`: double

## OrderTicketFocusControl

Constants that are used to set the focus when you open standard Order dialog or Position dialog.

```javascript
OrderTicketFocusControl.StopLoss = 1 // focus stop loss control
OrderTicketFocusControl.StopPrice = 2 // focus stop price for StopLimit orders
OrderTicketFocusControl.TakeProfit = 3 // focus take profit control
```

## Brackets

* `stopLoss`: double
* `takeProfit`: double

## Formatter

An object with `format` method that can be used to format the number to a string.
