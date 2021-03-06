## Mutable data item
For optimizing community new data searching, mutable data item of DHT is used as both publishing and requesting immutable data item. Each mutable data item includes two components:
* the target content: the key of the target immutable data item, which could be either a requesting or a publishing of tip block or new message
* a referral public key: for opitmizing the latest information searching, the public key is at nodes best knowledge of the latest peer that communicates
## Protocol of re-publish immutable item
According to the reading of `mutable request channel`, all nodes re-publish the immutable item which is stored locally. If not stored locally, nodes will put public-key of the requesting node into mutable referral public key and keep `target content` nil, then publish the mutable item into request channel.  
``` 
Mutable data item does not follow re-publish protocol
```
## Salt channels
* Tip channel: content is the latest tip hash when blockchain grows, the tip could be own block or other miner's block. Node A publish a new block, A put block key into mutable item, then publish both mutable tip channel and immutable item. 
* Requset channel: when A requests a block, A put a key into the channel mutable data, then publish it.
* The `content` of mutable item in different channels: 
    * `blkTip` channel, the tip block 
    * `blkRequest`, the block hash on demand
    <br><br>
    * `msgTip` channel, the latest own message hash and lastest community msg pubkey
    * `msgRequest` channel, the msg hash on demand
    <br><br>
    * `txTip` pool channel, it the highest tx fee transaction. 
    * `txRequest` channel, the tx data schema hash on demand
    
## Mutable data sync mode
A node will control mutable data DHT put and get directly, running as synchronized tight coupling mode. A node will rely on referral to get intelligence for searching. 
## Immutable data a-sync life cycle: 
* When nodes A want to get a immutable data with a key, A will always search local memory. If not found, A will put the key into mutable data item and publish the mutable request. A will then exit the life cycle to leave DHT engine to do DHT get and add into local memory. 
* When other peer B read a mutable item from request channel, if B has such hash immutable content locally, the B will re-publish the immutable content; if not, B will put public-key of requesting node A into own mutable referral component, the content part is nil, then publish it. <br><br>
```
Everything relating to mutable data put/get and immutable data put is controled by application-self in sync mode; immutable get is controlled by TAU DHT middleware.
```

## Nature of data
* mutable item is new data: tight coupling controled by main app. 
* immutable item is old data: TAU dht engine will do get and load into memory for app. 
