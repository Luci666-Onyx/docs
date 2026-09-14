# Source: https://www.tari.com/lessons/01_keys.html

About TariBuildCommunity

active miners

[Download](https://tari.com/downloads)

About TariBuildCommunity

active miners

[Download](https://tari.com/downloads)

active miners

[< Back to lessons](https://tari.com/lessons)![Introduction to Keys](https://tari.com/assets/lessons/img/learn-intro-to-keys.png)

# Introduction to Keys

By Cayle Sharrock

The [tari\_crypto](https://docs.rs/tari_crypto) crate carries the fundamental Tari cryptography primitives. It wraps the Ristretto elliptic curve, and provides ergonomic methods for using private and public keys, Pedersen commitments and digital signatures.

```
use tari_crypto::ristretto::{ RistrettoSecretKey as SecretKey, RistrettoPublicKey as PublicKey };
use tari_utilities::hex::Hex; use tari_crypto::keys::PublicKey as PK;

fn main() {
    // Create the secret key 1;
    let k = SecretKey::from_hex("0000000000000000000000000000000000000000000000000000000000000001").unwrap();
    // Generate the public key, P = k.G
    let pubkey = PublicKey::from_secret_key(&k);
    println!("{}", pubkey)
}
```

```
bec7f50a7307aff31eef64789bcd50e996e4b16b9f974cabef4800add830392f
```

![Learning the Tari Codebase](https://tari.com/assets/lessons/img/learn-the-tari-codebase.png)[Learning the Tari Codebase](https://tari.com/lessons/00_introduction) [Read More](https://tari.com/lessons/00_introduction)

![How to run a Tari Node on Windows 10](https://tari.com/assets/lessons/img/learn-how-tari-works-2.png)[How to run a Tari Node on Windows 10](https://tari.com/lessons/00a_Execute_Tari_Node_Windows_10) [Read More](https://tari.com/lessons/00a_Execute_Tari_Node_Windows_10)

![Adding Tari to Your Exchange](https://tari.com/assets/lessons/img/placeholder-thumbnail.jpg)[Adding Tari to Your Exchange](https://tari.com/lessons/09_adding_tari_to_your_exchange) [Read More](https://tari.com/lessons/09_adding_tari_to_your_exchange)