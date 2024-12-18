# CKB Dogecoin SPV Service

[![License]](#license)

> [!WARNING]
> This repository is still in the proof-of-concept stage.

A service, which synchronizes [Dogecoin] headers to a [Dogecoin SPV on CKB]
and provides an API to generate proofs for [Dogecoin] transactions so that
they can be verified on [CKB].

[License]: https://img.shields.io/badge/License-MIT-blue.svg

## Usage

With the command line option `-h`(alias of `--help`), help will be printed.

### JSON-RPC API Reference

- Method `getTxProof`

  Arguments:

  - `txid` (a hexadecimal string)

    A dogecoin transaction id.

    **No `0x`-prefix, as same as [the format in Bitcoin RPC APIs](https://developer.bitcoin.org/reference/rpc/gettxoutproof.html#argument-1-txids).**


  - `tx-index` (an unsigned integer)

    The index of a transaction in the block; starts from 0.

    **Service doesn't check it, just pack it into the final proof.**

  - `confirmations` (an unsigned integer)

    Represents the required acceptance of the transaction by the dogecoin network.

  Result:

  - `spv_client` ([type: `OutPoint`])

    An out point of a SPV client cell in CKB.

  - `proof` ([type: `JsonBytes`])

    The full proof of a dogecoin transaction, which could be verified with
    above SPV client in CKB.

  <details><summary>An example:</summary>


  Request:

  ```shell
  curl -X POST -H "Content-Type: application/json" \
      -d '{"jsonrpc": "2.0", "method":"getTxProof", "params": ["65a4a4dda2d3d166f0d1b704cc81d5a16d8100dde7d1b54ecbdff84c0695c01d", 1, 2], "id": 1}' \
      http://127.0.0.1:8888
  ```

  Output:
  ```json
  {
      "jsonrpc": "2.0",
      "result": {
          "proof": "0xab04000014000000180000001c000000b70000000100000030296700970000000400620083949a77260c82bf2903287231c354afad2cd0a971cafbbffb4b9ab0976e004e6ea38f9546e52907ded0167dbe4632e04eac03f666f438e00a10f9862dad0c8134ce53679ed7011eaaaadea502000000023d0fd493b1a29af42b2123c76df45bb30c23e04fd7d1b9e61f0f12d72ae38359c09012c454056ebb0d67ce884f0d956a3d518287a1138af33b96da33902228c401050e000000c0ed6600bf0d6700ff3efa49c6740000000000000000000000000000000000000000000000000000d04892e72081458102788f7f15ae1a680dcb856cbb2c3c3e9c10c9c987618c8bc00d6700bf1d6700d5ec83b645020000000000000000000000000000000000000000000000000000ee95f1b48aca0030e111ae036708dd485966a4ad4545937168b9bebf4cfe3f7fc01d6700bf2567001d43a445c3000000000000000000000000000000000000000000000000000000aa3f61063151d7237f3e19b3f5c5f0d9dc64542e580a7cbde3cf066a2d480645312967003129670013989700000000000000000000000000000000000000000000000000000000009a36955c4d4c28076d1129145901a2f27136317a3cb6ee8138f2961003499ffe3229670033296700bcd5660100000000000000000000000000000000000000000000000000000000d4cd48a01c67c9e9d4a625517cfd80dbb486e1132c5f72cea4aad3567d79ddd734296700372967000ca75e0300000000000000000000000000000000000000000000000000000000843d6fcb25cbad80f4422c9fb9f9aa33cc73b8506e233d7fbbe4db363ad82724382967003f2967004c00700200000000000000000000000000000000000000000000000000000000fdf38c8934f0999de3908b98dac6fc3f64a3101b751e2c93cdcdaf6e15e61fb3202967002f29670020c60b05000000000000000000000000000000000000000000000000000000007b5ae9a0a85aa4a7fb7f0a2f0ddbcfea7580aab1d9341bec8c95daade30ab921002967001f296700f2adccd00000000000000000000000000000000000000000000000000000000049da1c0e0831edc012392454cd8b67483ea97f274356a09ea47ef5b89020874cc0286700ff2867007c55995300000000000000000000000000000000000000000000000000000000ec5218acc03377dbbfb6ac64e5848fcafea7234b27ad8572a310c329515b036440296700bf296700b9b52163000000000000000000000000000000000000000000000000000000003554a3bf92629a02170d3b9ff3314192d3882a0a55226bb4f9b2b79ef9278757c0276700bf2867006cb9c78301000000000000000000000000000000000000000000000000000000eda32d67a4ddc86be8287899ca3a85ecd1de8eaf79e6a844adb47b13c73dc681c0256700bf276700c0d320f102000000000000000000000000000000000000000000000000000000f0023d5293eb57ab74ac1078f558a53466a5a11c4c6764e2dec468cb28dc9cf8c02967003a2a6700b5866fd10200000000000000000000000000000000000000000000000000000070688554c4d97e6f1c14572284f3f04e151b1ffc3c3d377e4d26c5085276186b",
          "spv_client": {
              "index": "0x1",
              "tx_hash": "0x60ee209b6ae4958b74ed15397258f477090287802a71d936b36b7662f4829d45"
          }
      },
      "id": 1
  }
  ```

  </details>

## Related Projects

- [The Core Library of CKB Dogecoin SPV]

- [The CKB Dogecoin SPV Contracts]

## License

Licensed under [MIT License].

[Dogecoin]: https://dogecoin.org
[Bitcoin]: https://bitcoin.org
[CKB]: https://github.com/nervosnetwork/ckb

[type: `OutPoint`]: https://github.com/nervosnetwork/ckb/tree/v0.114.0/rpc#type-outpoint
[type: `JsonBytes`]: https://github.com/nervosnetwork/ckb/tree/v0.114.0/rpc#type-jsonbytes

[MIT License]: LICENSE
