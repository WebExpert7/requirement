# Party Relationship Covenant

The Party Relations covenant is responsible for managing relations between parties. This includes providing the Global Directory with information about how other chains can reach out to this party for a relationship, and creating new relationships between the party it belongs to and another party. It creates new relationships by requesting them from a counterparty then gathering requisite information to facilitate the creation of a new UDO Instance Manager between the party and the counterparty.

> NOTE: For this version of the spec, relationship maintenance is not required. A simple interface for forwarding invitations is all that is required

### State Slices

| State Slice                                       | Usage                                                                                                             |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| [`directory`](directory-covenant.md#state-slices) | Consumed from the _Global Directory_, this is the shared calling card for how to reach other participating chains |
| `directoryEntry`                                  | The information that this party will send to the _Global Directory_                                               |
| `directoryEntry.chainId`                          | The chainId of this party's directory. The chain you must reach out to to form a relationship                     |
| `directoryEntry.partyName`                        | A friendly party name for the directory                                                                           |
| `directoryEntry.metaData`                         | An object containing context specific meta-data. This can be anything                                             |
| `directoryEntry.directoryPublicKeys`              | Approved public keys to authorize this party to run a shared *Global Directory* chain                             |
| `directoryEntry.relationshipPublicKeys`           | Approved public keys to authorize this party to run a shared *UDO Instance Manager* chain                         |



```js
const state = {
  directory: {
    shared: {
      sharedCovenantHash: 'fdrv9u8dj9esj90oj3rdo09jrf',
      relationshipPublicKeys: {
        x90rfjkwr30e9sada: [...pubKeys],
        ...
      },
    },
    parties: {
      chnid_90rfjkwr30e9: {
        chainId: 'chnid_90rfjkwr30e9',
        relationshipPublicKeys: [...pubKeys]
        ...
      },
      ...
    }
  },
  directoryEntry: {
    chainId: 'x90rfjkwr30e9sada',
    partyName: 'ACME Corp.',
    metaData: {
      accountNo: '321031209219',
      tel: '+1-542-5487'
    },
    relationshipPublicKeys: [...pubKeys],
    directoryPublicKeys: [...pubKeys]
  },
  invites: [{ inviterChainId: '', invitedPartyChainId: ''}, ...]
}
```

### Joins

> Note: The joins are not to implemented for this version of the spec. All that is required is an action/some extra state for testing that interactions with the data from joins that is provided by other chains is interacted with correctly in the selectors.

#### Provide

| Name                      | Consumer           | State Slice      |
| ------------------------- | ------------------ | ---------------- |
| `DIRECTORY_${ownChainId}` | _Global Directory_ | `directoryEntry` |

#### Consume

| Name                      | Provider           | Mount Slice |
| ------------------------- | ------------------ | ----------- |
| `DIRECTORY_${ownChainId}` | _Global Directory_ | `shared`    |

#### Send Action To

| Receiver           | Action       |
| ------------------ | ------------ |
| _Global Directory_ | INVITE_PARTY |

### Actions

#### h. `@@interbit/STROBE`

An updated timestamp. Store the time in timestamp and check on each PENDING relationship. If too much time has elapsed, cancel the relationship.

> Note: This is a system action that the mock test harness does not provide. You can test this by simply dispatching `Date.now()` with the correct action type.

##### Payload

- `timestamp`: A trustworthy timestamp

##### State Changes

- Store the timestamp in state

##### Tests

- The timestamp has been stored
- If the value is not a timestamp, it is ignored
- If the valus is not greater than the previous timestamp, it is ignored


#### j. `UPDATE_DIRECTORY_ENTRY`

Update the directory entry for this party.

##### Payload

-  `directoryEntry`: The values to add to the directory entry.

##### State Changes

- Merge the payload state over the store state.
- If a field is undefined, do not overwrite it.
- If a field is null or an empty string, overwrite it.

##### Tests

- Merge the payload state over the store state.
- If a field is undefined, do not overwrite it.
- If a field is null or an empty string, overwrite it.
- If directoryEntry is empty do nothing

#### k. `INVITE_REQUEST`

A User has indicated they wish to invite a party to the _Global Directory_. Forward the action to the Global directory and track the request in state.

##### Payload

-  `inviterChainId`: The chainId of the user chain that invited the other party to join
-  `invitedPartyChainId`: The chainID of the _Party Relationship_ chain that was invited to join the _Global Directory_.

##### State Changes

- Forwarded action appears in forward action queue
- Invite request was stored in state

##### Tests

- Forwarded action appears in forward action queue
- Invite request was stored in state
- If inviterChainId is undefined do nothing
- If invitedPartyChainId is undefined do nothing


### Selectors

> State should always be the first parameter in selector functions

| Selector                            | Purpose                                                                                |
| ----------------------------------- | -------------------------------------------------------------------------------------- |
| `getSharedCovenantHash`             | Gets the shared covenant hash from the provided state from the _Global Directory_      |
| `getSharedRelationshipPublicKeys`   | Gets the relationship public keys  from the provided state from the _Global Directory_ |
| `getDirectoryEntryForParty`         | Gets the entire directory entry from the provided state from the _Global Directory_    |
| `getChainIdForParty`                | Gets the chain ID for a party from the provided state from the _Global Directory_      |
| `getPartyNameForParty`              | Gets the name of the party from the provided state from the _Global Directory_         |
| `getMetaDataForParty`               | Gets the party's meta-data from the provided state from the _Global Directory_         |
| `getRelationshipPublicKeysForParty` | Gets the relationship public keys from the provided state from the _Global Directory_  |
| `getDirectoryPublicKeysForParty`    | Gets the directory public keys from the provided state from the _Global Directory_     |
| `getDirectoryEntry`                 | Gets the entire directory entry from the `directoryEntry` part of state                |
| `getChainId`                        | Gets the chain ID for a party from the `directoryEntry` part of state                  |
| `getPartyName`                      | Gets the name of the party from the `directoryEntry` part of state                     |
| `getMetaData`                       | Gets the party's meta-data from the `directoryEntry` part of state                     |
| `getRelationshipPublicKeys`         | Gets the relationship public keys from the `directoryEntry` part of state              |
| `getDirectoryPublicKeys`            | Gets the directory public keys from the `directoryEntry` part of state                 |
| `getInvites`                        | Gets the list of invitations that have been sent to the _Global Directory_             |


### Tests

- a positive test for each selector
- Test that each selector that returns an array type returns [] if data unavailable
- test that each other type type selector returns undefined if data unavailable
