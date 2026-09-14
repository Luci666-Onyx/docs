# Source: https://tari.com/lessons/05_emoji_id.html

About TariBuildCommunity

active miners

[Download](https://tari.com/downloads)

About TariBuildCommunity

active miners

[Download](https://tari.com/downloads)

active miners

[< Back to lessons](https://tari.com/lessons)![Working with Emoji Id](https://tari.com/assets/lessons/img/learn-emoji-id.png)

# Working with Emoji Id

By Cayle Sharrock

The `EmojiID` struct in the [`tari_wallet` crate](https://docs.rs/tari_wallet) provides everything you need to work with Tari's Emoji ID. In this tutorial, we will learn how to create an Emoji ID from a public key and vice versa. We will also validate an emoji ID against transcription errors.

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

![How Tari Works - Part II](https://tari.com/assets/lessons/img/learn-how-tari-works-2.png)[How Tari Works - Part II](https://tari.com/lessons/04_how_tari_works_ii) [Read More](https://tari.com/lessons/04_how_tari_works_ii)

![Signing a Message](https://tari.com/assets/lessons/img/learn-signing-a-message.png)[Signing a Message](https://tari.com/lessons/03_signatures) [Read More](https://tari.com/lessons/03_signatures)

![How Tari Works - Part I](https://tari.com/assets/lessons/img/learn-how-tari-works.png)[How Tari Works - Part I](https://tari.com/lessons/02_how_tari_works) [Read More](https://tari.com/lessons/02_how_tari_works)