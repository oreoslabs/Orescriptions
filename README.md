Orescriptions Specification
==================

This is the Orescriptions specification, a repository that contains core orescriptions protocol. 

Orescriptions has been deployed on [IronFish mainnet](https://www.orescriptions.com/) and [IronFish testnet](https://testnet.orescriptions.com/).

**Introduction**

Orescriptions, inspired by Bitcoin Inscriptions, is a way creating and sharing NFTs on Iron Fish blockchain. Orescriptions protocol uses **Iron Fish native multi-assets mint transaction** to mint NFT, then this NFT can be transfered privately. Orescriptions protocol declares some constraints as follows, only mint transaction meets the protocol will be tracked on the dashboard, and sendable/tradable in the future.

**Native Multi-Asset Mint**

Iron Fish native mint request contains name, value, metadata of the asset to be minted as follows.
```
interface MintAssetRequest {
  account?: string
  fee?: string
  value: string
  assetId?: string
  expiration?: number
  expirationDelta?: number
  confirmations?: number
  metadata?: string
  name?: string
  transferOwnershipTo?: string
}
```

**Orescriptions NFT Mint**

Orescriptions protocol declares constraints as follows to distinguish NFT with native multi-assets.

1. `value = 1` to confirm this NFT is unique
2. `name = orescriptions` to be tracked as orescriptions 
3.  `metadata` is string form of `OrescriptionsDeploy | OrescriptionsMint` object
4. `assetId` is empty to confirm this NFT is new minted instead of inflation

```
type OrescriptionsDeploy = {
  tick?: string; // collection name used to classify to its collection, 4 characters only
  op: 'deploy';
  max: number; // the total mintable amount for this collection
};

type OrescriptionsMint = {
  tick?: string; // collection name used to classify to its collection
  op: 'mint';
  data: string; // data cid on ipfs
};
```

**Orescriptions NFT Transfer**

Same as Iron Fish native multi-assets, orescriptions NFT can be sent to others privately via native transaction. To transfer the NFT, 
1. `transaction assetId = NFT assetId`
2. `transaction amount = 1`

**Orescriptions NFT Burn**

Same as Iron Fish native multi-assets, orescriptions NFT can be burned via native transaction. To burn the NFT,
1. `transaction assetId = NFT assetId`
2. `transaction amount = 1`

**Example**

1. deploy

```
const oregenesis: OrescriptionsDeploy = {
  tick: 'Ores',
  op: 'deploy',
  max: 1000,
};
const nativeRequest: MintAssetRequest = {
  account: 'default',
  fee: '1',
  value: '1',
  metadata: JSON.stringify(oregenesis),
  name: 'orescriptions',
};
```

2. mint

```
const oregenesis: OrescriptionsMint = {
  tick: 'Ores',
  op: 'mint',
  data: '', // not real
};
const nativeRequest: MintAssetRequest = {
  account: 'default',
  fee: '1',
  value: '1',
  metadata: JSON.stringify(oregenesis),
  name: 'orescriptions',
};
```