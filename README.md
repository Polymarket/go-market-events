# Markets events

## How to run

```
make run
```

## Events

### Proposed price

When someone thinks a market is ready to be resolved, proposes a price. This means that the market is close to be resolved.
This action emits this event:

```solidity

event ProposePrice(
    address indexed requester,
    address indexed proposer,
    bytes32 identifier,
    uint256 timestamp,
    bytes ancillaryData,
    int256 proposedPrice,
    uint256 expirationTimestamp,
    address currency
);
```

#### How to relate this event with a market in the API

- Parse the `ancillaryData` to question id

```go
questionId := crypto.Keccak256Hash(log.AncillaryData)
```

- Then use that value here:

```
curl --location 'https://clob.polymarket.com/markets-by-question-id/{question id}'
```

### Disputed price

If the resolution is accepted the market gets resolved and that's all. However, if someone disagres is possible to dispute the proposed resolution. This means that the resolution will take a bit longer.

```solidity
event DisputePrice(
    address indexed requester,
    address indexed proposer,
    address indexed disputer,
    bytes32 identifier,
    uint256 timestamp,
    bytes ancillaryData,
    int256 proposedPrice
);
```

#### How to relate this event with a market in the API

- Parse the `ancillaryData` to question id

```go
questionId := crypto.Keccak256Hash(log.AncillaryData)
```

- Then use that value here:

```
curl --location 'https://clob.polymarket.com/markets-by-question-id/{question id}'
```

### Condition resolved

When the market actually gets resolved, this event is emitted:

```solidity
event ConditionResolution(
    bytes32 indexed conditionId,
    address indexed oracle,
    bytes32 indexed questionId,
    uint outcomeSlotCount,
    uint[] payoutNumerators
);
```

#### How to relate this event with a market in the API

- Use the `conditionId` field to filter in the API request

```
curl --location 'https://clob.polymarket.com/markets/{condition id}'
```

\*\*Check `watcher/watcher.go` to see how to parse the incoming events
