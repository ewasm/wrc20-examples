# WRC-20 challenge examples

This repository contains examples of WRC20 tokens written in different languages.

## Joining the challenge

Have a look at [this gist](https://gist.github.com/axic/16158c5c88fbc7b1d09dfa8c658bc363), implement the logic for the following contract (pseudocode) in your favorite language:

```javascript
// main ewasm entry point
function main() { 
  if (eei_calldatasize() < 4)
    eei_revert(0, 0)
  let selector = eei_calldatacopy(0, 4)
  switch selector {
    case 0x9993021a:
      do_balance()
    case 0x5d359fbd:
      do_transfer()
    default:
      eei_revert(0, 0)
  }
}

function do_balance() {
  if (eei_calldatasize() != 24)
    eei_revert(0, 0)

  let address = eei_calldatacopy(4, 20)
  // make sure that address is 160 bits here,
  // but storage key is 256 bits so need to pad it somehow
  let balance = eei_storageload(address)
  eei_return(balance)
}

function do_transfer() {
  if (eei_calldatasize() != 32)
    eei_revert(0, 0)

  let sender = eei_sender()
  let recipient = eei_calldatacopy(4, 20)
  let value = eei_calldatacopy(24, 8)
  let sender_balance = eei_storageload(sender)
  let recipient_balance = eei_storageload(recipient)
  
  if (sender_balance < value)
    eei_revert(0, 0)

  sender_balance -= value
  recipient_balance += value

  eei_storagestore(sender, sender_balance)
  eei_storagestore(recipient, recipient_balance)
}
```

Then simply put it in a directory named after your language and send us a PR.
