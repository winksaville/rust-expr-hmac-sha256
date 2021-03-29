# Experiment with hmac-sha256

Validate that the hmac-sha256 crate generates signatures
that match what binance.us expects.

The src/main.rs:
```
/// Test that hmac_sha256 produce the expected results
/// as defined by [binance.us api docs](https://github.com/binance-us/binance-official-api-docs).

use hmac_sha256::HMAC;

#[macro_use]
extern crate hex_literal;


fn main() -> () {
    // Data, key and expected
    // from: https://github.com/binance-us/binance-official-api-docs/blob/fc916164ae04eb2e95ff7f98c2d49d8d6bd6d096/rest-api.md#example-2-as-a-query-string
    let data = b"symbol=LTCBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1499827319559";
    let key = b"NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j";
    let expected = hex!("c8db56825ae71d6d79447849e617115f4a920fa2acdcab2b053c4b2838bd6b71");

    // Calculate the signature from the data and key
    let signature = HMAC::mac(data, key);
    println!("signature ={:02x?}", signature);

    // Validate
    assert_eq!(signature, expected);
}

```

## Run

It works!
```
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/expr-hmac-sha256`
signature =[c8, db, 56, 82, 5a, e7, 1d, 6d, 79, 44, 78, 49, e6, 17, 11, 5f, 4a, 92, 0f, a2, ac, dc, ab, 2b, 05, 3c, 4b, 28, 38, bd, 6b, 71]
```
