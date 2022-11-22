# Void wallet
A work-in-progress bitcoin wallet that only sends and receives coinjoins

# What is this?

This is a bitcoin wallet designed around the idea of making every spend a coinjoin. I want it to do a coinjoin whenever you send money and I want it to only accept money from wallets who sent the money inside a coinjoin.

# Why did you do this?

Mostly to dunk on monero people. Also because I care about people's privacy, especially my own, and current coinjoin wallets are too hard to use, I want something simple and easy, (I know, that's not what this wallet is yet, but I think the foundations are there. Have some patience and go use some other coinjoin wallet while you wait.) Monero people show up in my twitter feed all the time touting their block explorer where each transaction hides the sender and the recipient. Well I figure if I make a wallet that only uses coinjoins, we can dunk back on them by showing them that bitcoin has the same thing. Use void wallet and all your transactions will hide the sender and the recipient, just like in monero.

Also, I'm not satisfied with the way coinjoins are done in bitcoin and I want to improve them. My chief criticisms are: the coordinators in samourai and wasabi can be targeted by law enforcement, which may scare people away from running the coordination software, and samourai and wasabi also prioritize or require rounded coinjoin amounts like 1 million sats, 100,000 sats, or 10 million sats, which makes it hard to use coinjoins for everyday payments. For those of you pointing out that joinmarket never had these problems, I know, I love joinmarket, but I don't think any current version of it works well on mobile, so it doesn't escape my criticism either.

# Oh, I thought you did this because you don't like the fee structure in other coinjoin implementations

Oh yeah, I forgot about that. Samourai, wasabi, and joinmarket all make regular users pay someone to do a coinjoin, and samourai and wasabi also make you pay the coordinator when you want to add funds to the coinjoin pool. That all seems like maybe it's overkill. Ok it's probably not, there are good reasons for doing that, let me rephrase. In samourai and wasabi, there is a semi-constant trickle of coinjoins every block or two, and people pay money to park their coins in that trickle and just basically coinjoin all the time. The same people, if they used joinmarket, would *get paid* to do that. And *both* of those models are popular with users. Isn't that a bit weird? Why are people happy to *pay to constantly coinjoin* when they can *get paid* to constantly coinjoin? I'm not sure, but it has a consequence that if I make that part free, I can use a "save more money" pitch in both directions: if you want to park your coins somewhere where they will constantly coinjoin, mine is free, unlike samourai and wasabi. And if you just want to click "send" and know a coinjoin happened, void wallet is cheaper than any of the others, because in void wallet you just pay a mining fee, you don't have to pay a coordinator's fee or a market maker's fee.

# Who is the coordinator in void wallet?

Your wallet acts as the coordinator whenever you hit the Send button. It creates a psbt, goes out to find other people who use this wallet, and invites them to be in the coinjoin. (Actually that statement is aspirational -- right now you have to manually find people to do a coinjoin with and type in their pubkeys. Give it time.) Then you pass your psbt back and forth with your selected coinjoiners until everyone has contributed some utxos and cosigned it. Then you broadcast the signed transaction. Since *your wallet* is the coordinator for *your transactions,* you don't have to pay anyone to do the coordination for you, and law enforcers have no central intermediary to target for enforcement. They'll never know who is coordinating what!

# Why does the send page have two send buttons?

Because this wallet isn't done yet. The first send button doesn't do a coinjoin and will soon be removed. The second one does do a coinjoin but right now you have to fill in some pubkeys first, which is difficult because it's not supposed to expose that to the user, your wallet is supposed to fill that out automatically but I haven't implemented that yet. When I do, the form will disappear and there will just be a nice pretty Send button.

# Why is this wallet the ugliest wallet I've ever seen?

It's not, it's stark and simple, which is only ugly to *some* people, not to me. The reason why it's stark and simple is because it's not done yet. I based it on [this project](https://github.com/supertestnet/vanilla-js-browser-wallet) in which I tried to make a general purpose wallet that any developer can customize to look how they want. I haven't customized this new wallet yet so it looks pretty much exactly like its dad. When all the functionalithy I want is present I'll try to make it prettier. (2 weeks TM.)

# You said earlier that this wallet canonly receive money if it's in a coinjoin. Can you say more about that?

That's aspirational too. Right now this wallet receives money the normal way -- you show someone a bitcoin address and they can send money to it, bada boom, bada bing, you got some sats. But I built in the same functions that I use in [my coinjoin explorer](https://github.com/supertestnet/coinjoin-explorer) and when I turn that feature on the wallet will only *see* transactions if they are coinjoin transactions. So if someone tries to send you money using a normal bitcoin wallet, this one won't know about it.

# Won't that be confusing? Will some people lose money?

Probably not because before I release this wallet, I intend to modify the address format so that ordinary bitcoin wallets won't recognize this wallet's address format. That way they won't accidentally send you money in a transaction that your wallet can't recognize. My goal is to have it so that people can only send money into this wallet via a coinjoin, and once it's in a coinjoin, it will stay in a long series of coinjoins and hopefully never go back into a normal bitcoin address again. If I reach my goal, this wallet will interact with a subgraph of the bitcoin network that is all coinjoins all the time. Privacy ON -- by default -- no exceptions. That's my goal. Here's hoping we get there.
