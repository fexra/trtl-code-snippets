# Decode Integrated Address

Find the payment ID of an integrated address.


## Code

```javascript
var decodedAddress = Base58.decode(recipient)
    var encodedPrefix = CryptoNote.encode_varint(ExplorerConfig.addressPrefix)
    var prefix = decodedAddress.slice(0, encodedPrefix.length)
    
    if (prefix !== encodedPrefix) {
      setRecipientAddressState(true)
      return
    }
    
    decodedAddress = decodedAddress.slice(encodedPrefix.length)
    
    /* This usually means that we have an integrated address on our hands */
    if (decodedAddress.length > 136) {
      var paymentId = decodedAddress.slice(0, 128)
      decodedAddress = decodedAddress.slice(128)
    } else {
      var paymentId = ''
    }
    
    var publicSpend = decodedAddress.slice(0, 64)
    var publicView = decodedAddress.slice(64, 128)
    var checksum = decodedAddress.slice(-8)
    
    if (paymentId.length === 0) {
      var expectedChecksum = CryptoNote.cn_fast_hash(prefix + publicSpend + publicView).slice(0, 8)
    } else {
      var expectedChecksum = CryptoNote.cn_fast_hash(prefix + paymentId + publicSpend + publicView).slice(0, 8)
      paymentId = Base58.hextostr(paymentId)
    }
    
    if (expectedChecksum !== checksum) {
      throw 'Could not parse address'
    }
    ```

    ## Credits

    - iburnmycd