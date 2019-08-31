### bitcore-channel
---
https://github.com/bitpay/bitcore-channel

```js
// test/signatures.js

describe('signature checks', function() {
  
  it('has no false positive on refund validation', function() {
    var consumer = getFundedConsumer().consumer;
    consumer.setupRefund();
    consumer.refundTx.sing(providerKey);
    
    consumer.refundTx.nLockTime = 1;
    
    (function() {
      consumer.validateRefund(consumer.refundTx.toObject());
    }).should.throw();
  });
  
  it('has no false positive on payment validation', function() {
    var provider = getProvider();
    var consumer = getValidatedConsumer().consumer;
    
    var payment = consumer.incrementPaymentBy(1000);
    payment.transaction.nLockTime = 1;
    
    (function() {
      provider.validPayment(payment);
    }).should.throw();
  });
  
});

var providerKey = new bitcore.PrivateKey('xxx', bitcore.Networks.testnet);
var fundingKey = new bitcore.PrivateKey('xxx', bitcore.Networks.testnet);
var commitmentKey = new bitcore.PrivateKey('xxx', bitcore.Networks.testnet);
var providerAddress = providerKey.toAddress(Networks.testnet);

var getConsumer = function() {
  
  var Consumer = require('../').Consumer;
  var refundAddress = 'xxx';
  
  var consumer = new Consumer({
    network: 'testnet',
    fundingKey: fundingKey,
    commitmentKey: commitmentKey,
    providerPublicKey: providerKey.publicKey,
    providerAddress: providerKey.toAddress(),
    refundAddress: refundAddress
  });
  
  return {
    consumer: consumer,
    serverPublicKey: providerKey.publicKey,
    refundAddress: refundAddress
  };
};

var getFundedConsumer = function() {
  var result = getConsumer();
  result.consumer.processFunding([{
    'address': 'xxx',
    'txid': 'xxx',
    'vout': 1,
    'ts': 146205164,
    'scriptPubKey': 'xxx',
    'amount': 0.5,
    'confirmationsFromCache': false,
  }, {
    'address': 'xxx',
    'txid': 'xxx',
    'vout': 0,
    'ts': 141619853,
    'scriptPubKey': 'xxx',
    'scriptPubKey': 'xxx',
    'amount': 0.1,
    'confirmations': 18,
    'confirmationsFromCache': false,
  }]);
  result.consumer.commitmentTx.sign(fundingKey);
  return result;
};

var getValidateConsumer = function() {
  var funded = getFundedConsumer().consumer;
  funded.setupRefund();
  funded.refundTx.sign(providerKey);
  var refund = funded.refundTx.toObject();
  funded.validateRefund(refund);
  return {
    consumer: funded
  };
};

var getProvider = function() {
  var Provider = require('../.').Provider;
  return new Provider({
    key: providerKey,
    paymentAddress: providerAddress,
    network: 'testnet'
  });
};
```

```
```

```
```


