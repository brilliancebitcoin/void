# Void wallet
A work-in-progress bitcoin wallet that only sends and receives coinjoins

# Video demo

[![](https://brilliancebitcoin.github.io/void/void-wallet-with-youtube-logo.png)](https://www.youtube.com/watch?v=Oi4qEKtkabE)

# How to try

Click here: https://brilliancebitcoin.github.io/void/

# What is this?

This is a bitcoin wallet designed around the idea of making every spend a coinjoin. I want it to do a coinjoin whenever you send money and I want it to only accept money from wallets who sent the money inside a coinjoin.

# Why did you do this?

Mostly to dunk on monero people. Also because I care about people's privacy, especially my own, and current coinjoin wallets are too hard to use, I want something simple and easy, (I know, that's not what this wallet is yet, but I think the foundations are there. Have some patience and go use some other coinjoin wallet while you wait.)

Monero people show up in my twitter feed all the time touting their block explorer where each transaction hides the sender and the recipient. Well I figure if I make a wallet that only uses coinjoins, we can dunk back on them by showing them that bitcoin has the same thing. Use void wallet and all your transactions will hide the sender and the recipient, just like in monero.

Also, I'm not satisfied with the way coinjoins are done in bitcoin and I want to improve them. My chief criticisms are: the coordinators in samourai and wasabi can be targeted by law enforcement, which may scare people away from running the coordination software, and samourai and wasabi also prioritize or require rounded coinjoin amounts like 1 million sats, 100,000 sats, or 10 million sats, which makes it hard to use coinjoins for everyday payments. For those of you pointing out that joinmarket never had these problems, I know, I love joinmarket, but I don't think any current version of it works well on mobile, so it doesn't escape my criticism either.

# Oh, I thought you did this because you don't like the fee structure in other coinjoin implementations

Oh yeah, I forgot about that. Samourai, wasabi, and joinmarket all make regular users pay someone to do a coinjoin, and samourai and wasabi also make you pay the coordinator when you want to add funds to the coinjoin pool. That all seems like maybe it's overkill. Ok it's probably not, there are good reasons for doing that, let me rephrase.

In samourai and wasabi, there is a semi-constant trickle of coinjoins every block or two, and people pay money to park their coins in that trickle and just basically coinjoin all the time. The same people, if they used joinmarket, would *get paid* to do that. And *both* of those models are popular with users. Isn't that a bit weird? Why are people happy to *pay to constantly coinjoin* when they can *get paid* to constantly coinjoin? I'm not sure, but it has a consequence that if I make that part free, I can use a "save more money" pitch in both directions:

If you want to park your coins somewhere where they will constantly coinjoin, void wallet gives you that for free, unlike samourai and wasabi. And if you just want to click "send" and know a coinjoin happened, void wallet is cheaper than any of the others, because in void wallet you just pay a mining fee, you don't have to pay a coordinator's fee or a market maker's fee.

# Who is the coordinator in void wallet?

Your wallet acts as the coordinator whenever you hit the Send button. It creates a psbt, goes out to find other people who use this wallet, and invites them to be in the coinjoin. (Actually that statement is aspirational -- right now you have to manually find people to do a coinjoin with and type in their pubkeys. Give it time.) Then you pass your psbt back and forth with your selected coinjoiners until everyone has contributed some coins and cosigned the transaction. Then you broadcast the signed transaction. Since *your wallet* is the coordinator for *your transactions,* you don't have to pay anyone to do the coordination for you, and law enforcers have no central intermediary to target for enforcement. They'll never know who is coordinating what!

# Why is this wallet the ugliest wallet I've ever seen?

It's not, it's stark and simple, which is only ugly to *some* people, not to me. The reason why it's stark and simple is because it's not done yet. I based it on [this project](https://github.com/supertestnet/vanilla-js-browser-wallet) in which I tried to make a general purpose wallet that any developer can customize to look how they want. I haven't customized this new wallet yet so it looks pretty much exactly like its dad. When all the functionalithy I want is present I'll try to make it prettier. (2 weeks TM.)

# You said earlier that this wallet can only receive money if it's in a coinjoin. Can you say more about that?

That's aspirational too. Right now this wallet receives money the normal way -- you show someone a bitcoin address and they can send money to it, bada boom, bada bing, you got some sats. But I built in the same functions that I use in [my coinjoin explorer](https://github.com/supertestnet/coinjoin-explorer) and when I turn that feature on the wallet will only *see* transactions if they are coinjoin transactions. So if someone tries to send you money using a normal bitcoin wallet, this one won't know about it.

# Won't that be confusing? Will some people lose money?

Probably not because before I release this wallet, I intend to modify the address format so that ordinary bitcoin wallets won't recognize this wallet's address format. That way they won't accidentally send you money in a transaction that your wallet can't recognize. My goal is for people to only send money into this wallet via a coinjoin, and once that money is in a coinjoin, it should stay in a long series of coinjoins and hopefully never go back into a normal bitcoin address ever again. If I reach my goal, this wallet will interact with a subgraph of the bitcoin network that consists of all coinjoins (and only coinjoins) all the time. Privacy ON -- by default -- no exceptions. That's my goal.

# A new address format? And it will be *required*? That sounds kind of bad. What if I want to send some coinjoined sats to someone who doesn't have this wallet? Or to a service like coinbase that won't accept coinjoined coins? Can I never get my money out of the void?

By design, no, my hope is that money that goes into a void wallet stays among void wallets forever. (It can go from one void user to another, or to other wallets and services that add support for my new privacy-focused address format, but that's it. And I don't think services like coinbase will ever add support for this format, because they don't treat privacy very kindly, so don't expect to ever send money that goes into the void to a service like coinbase.) This wallet is for people who want decent privacy all the time so they don't have to think about it. If someone uses some other wallet or service, this wallet just won't send to those people. (Well, right now it can, but that feature will be removed and this wallet will eventually only send to wallets that support the new privacy-focused address format I talked about earlier.)

A consequence of enforcing privacy is leaving behind surveillance-friendly wallets and services. If you want to send someone money using this wallet, this wallet wants confidence that the money will stay coinjoined. The new address format I hope to unveil soon helps with that, so as soon as I make it, I want it to be a requirement in this wallet.

# "Surveillance-friendly" seems like a loaded term when you really mean "anyone who doesn't support my funky new address format." Are you saying samourai and wasabi and other coinjoin wallets are surveillance friendly? What if I want to send money from this wallet to those wallets?

No, I'm not saying that. But this wallet won't be able to send money to those wallets unless they add support for the new address format I'm releasing.

# That seems stupid, why are you trying to push everyone to use some weird new address format of your own design?

Because I have a theory. I think that people who use coinjoin wallets get worried about things like this: oh no, you accidentally mixed toxic change with your coinjoined utxos, and now they're easy to correlate. Oh no, three of the people you coinjoined with moved their money straight to coinbase after the coinjoin, doxxing themselves and thus destroying your anonymity set. And on and on it goes. There are too many things that can go wrong if your wallet doesn't enforce any kind of limits. So for me, one of those limits is by requiring an address format that, at least initially, I know will only be used by this wallet, which knows how to do coinjoins properly and how to avoid mixing toxic change and which -- because of the unique address format -- can't even send money to coinbase. That very limitation is a strong privacy protection. But also, I think it will bring peace of mind. If you actually find someone else who can give you an address format that this wallet can send to, you probably don't have to worry anymore -- their wallet knows how to do things right, so your anonymity set will probably stay strong, so you can rest easy. Easy -- privacy isn't easy yet, but maybe someday it will be.
