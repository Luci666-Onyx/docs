---
sidebarTitle: "Working with Emoji ID"
title: "Working with Emoji ID"
---

# Working with Emoji ID

By Cayle Sharrock

The `EmojiID` struct in the `tari_wallet`[ crate](https://docs.rs/tari_wallet) provides everything you need to work with Tari's Emoji ID. In this tutorial, we will learn how to create an Emoji ID from a public key and vice versa. We will also validate an Emoji ID against transcription errors.

```
use tari_wallet::util::emoji::EmojiId;
use tari_crypto::tari_utilities::hex::Hex;

fn main() {
    const EMOJI: &str = "🐎🍴🌷🌟💻🐖🐩🐾🌟🐬🎧🐌🏦🐳🐎🐝🐢🔋👕🎸👿🍒🐓🎉💔🌹🏆🐬💡🎳🚦🍹🎒";
    const EMOJI_SHORT: &str = "🐎🍴🌷🌟💻🐖🐩🐾🌟🐬🎧🐌🏦🐳🐎🐝🐢🔋👕🎸👿🍒🐓🎉💔🌹🏆🐬💡🎳🚦🍹";

    // Convert a public key into its emoji ID
    let eid = EmojiId::from_hex("70350e09c474809209824c6e6888707b7dd09959aa227343b5106382b856f73a").unwrap();
    println!("{}",eid);

    // Convert an emoji to public key (in hex)
    let pubkey = EmojiId::str_to_pubkey(EMOJI).unwrap().to_hex();
    println!("{}", pubkey);

    //Test if both constants declared at the top are valid
    assert!(EmojiId::is_valid(EMOJI));
    assert_eq!(EmojiId::is_valid(EMOJI_SHORT), false, "Missing checksum");
    // TODO - check that emoji ID protects against transcription errors
    println!("It's all good!");
}
```

```
🖖🥴😍🙃💦🤘🤜👁🙃🙌😱🖐🙀🤳🖖👍✊🐈☂💀👚😶🤟😳👢😘😺🙌🎩🤬🐼😎🥺
70350e09c474809209824c6e6888707b7dd09959aa227343b5106382b856f73a
It's all good!
```
