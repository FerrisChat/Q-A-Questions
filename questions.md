# Q&A Questions and Answers

### Would there be any type of system once a user hit a ratelimit on an endpoint too many times that it turns global
(from Drx)

Currently, there are no plans to do so, as we don't have global ratelimits in the server.
This may change in the future (mainly depending on how ~~poorly~~ nicely you all use the API), but you'd still need a *lot* of requests to hit this (most likely somewhere in the range of 2,500 to 7,500 429s in a 10 minute period).

### What parts of FerrisChat will be similar to Discord, and what parts will be significantly different?
(from smallpepperz)

We're trying to keep it somewhat similar to not confuse those coming from Discord, but there are already a few big changes:
* snowflakes: unsigned 128 bit ints, unlike Discord's unsigned 64 bit
* a lot of API routes are similar to Discord, but a lot of them are also completely different
* and of course, a dev team that actually listens to people and doesn't screw them over with random changes

### What are some benefits that donaters will have?
(from Ryland)

Probably nothing. Except a badge. If (and only if) we absolutely need to, we'll only add new features (not big features, only small ones) that require donating.

### When do you guys think this project will be finished?
(from Chasee)

No comment. We don't want to give an improper estimate and then disappoint people because it was not correct.

### What was the motive behind ferris chat? How will it be different from other platforms?
(from MrKomodoDragon)

For me (0/0) it was based off the fact Electron is... horrible at first, and later on when Discord introduced the message intent, I got fed up enough and started working on it.

For me (hydro), the chain events that is outlined in [Danny's Gist](https://gist.github.com/Rapptz/4a2f62751b9600a31a0d3c78100287f1) made me want something better than Discord. The poor, rude, and almost incoherent decisions by Discord's staff members made it seem like it was no longer a community building software run by a small company who listens to their users. It started to seem more like a corporation with some sort of monetary goal (proved by their pushing of nitro).

As for differences from other platforms... well... nothing much. Two biggest differences would be the fact it's entirely open source, and has a official app not written with Electron.

### Will there be any ratelimit bucket header?
(from Cryptex)

Yup. Can't give you a name *yet* but there'll be two:
* one specifying total requests
* one specifying number of seconds for those total requests

The body of 429 already has a JSON response that if I remember correctly wraps these two up and adds a `retry_after` field.

### What will the source of emoji be? (Eg Discord uses Twemoji)
(from jay3332)

Probably also Twemoji. Thanks to Izmoqwy's digging, we did find out that it might not be so easy to render it, as the built-in text renderer in `iced` makes you pick one font for a widget.

### Will FerrisChat be centralized (like Discord, Slack, Facebook, etc.) or decentralized (IRC, Matrix, etc.)?
(from Sophon)

Centralized, but if you so desired (later on, right now a lot of things are hardcoded) it'd be easy to set up your own instance. You wouldn't be able to easily set up your own clients tho, since a lot of them are probably hardcoded with the main API.

### Will there be an official client?
(from Sophon)

Yup, also Rusty. Using either `fltk` or `gtk-rs` and, depending on how hard it is to implement, allowing you to switch remote servers from the official servers to any selfhosted variant.

### Will there be a mobile app in the App Store for FerrisChat?
(from Al3amode)

valkyrie_pilot is currently working on one. Send condolences to them for needing to deal with Swift.

### How will permissions work?
(from hydro)

Pretty similar to Discord. You get a set of bitflags (made with Rust's `bitflags` crate, fwiw), and each bit corresponds to a permission the user has.
If you hit a endpoint that needs permissions, server goes to Redis hoping your permissions for that channel/guild are cached, otherwise falls back to DB.
If it turns out you don't have perms, you get hit with good old 403 Forbidden.
When a user changes permissions, it'll be instantly removed from Redis, leaving the next call to fall back.

### To which extent does it try to mimic Discord, what are the "will definitely do"s and "won't do"s?
(from Izmoqwy)

This is sort of a duplicate of smallpepperz's question above, and I can't really think of much else to add to it.

### The server is made to be self-host-able, right? If so, will the client have a "list of servers" where you can join guilds on multiple servers in the same client or will self-hosting require running a second instance of the client?
(from Izmoqwy)

For the official client, it'd depend on how easy it is to implement it. 
If it takes only a little of tweaking, you'd be able to easily connect to multiple nodes in a single client instance, but perhaps not all at the same time.
If it, on the other hand, takes a *lot* of tweaking, most likely you'd have to edit the hardcoded API URL and run a new instance.
This wouldn't (hopefully) be as much of a problem as it is with Discord and Electron in general, thanks to Rust being so lightweight.

### In the continuity of self-host question, will there be "server groups" for sysadmins? And how will server-wise management work altogether?
(from Izmoqwy)

There will be an official server on the official server for selfhosting support. 
As for server-wise management, I didn't have many plans to implement those at the start, but since you asked it, here goes.
I've been considering perhaps adding flags on admins that would basically let them bypass all permissions. This could be a *huge* issue tho, so this probably won't happen.
Restricting some app-wide features (ie creating guilds) to only authorized users seems like it could be easily implemented, and without much in the way of security issues.
Right now those are sort of the biggest things I can think of that would need to be globally controlled. Most of the rest can be controlled at a guild level.
If there's anything else you think of, ping me in chat.

### Are there plans for bots? If so, would they work the same way as with Discord? Because it can be an issue with selfhosted servers who will have a different endpoint, they could be "mods" though with a lua impl or something
(from Izmoqwy)

Bots are a yes.
Same way as Discord as well.
The biggest issue with selfhosting is defo going to be the different endpoint.
Unfortnately, I see no way around this, but perhaps we could require a parameter in API libs that optionally allows a custom server endpoint to be set up.
It might be difficult for some libs to dynamically set an endpoint, which is why I haven't already done so.

### Will ferris chat support RPC?
(from Littie6amer)

Rich presence is a good idea, I personally do like it, it might be added, but it won't be high priority.

### Do you foresee any trademark issues with using Ferris as your mascot?
(from smallpepperz)

Nope. Ferris is licensed under Creative Commons 0, or in other words, Ferris is dedicated to the public domain. Check the footer of https://rustacean.net.

### How do you plan to "advertise", or in other words get users to use FerrisChat?
(from jay3332)

For me (0/0) personally I don't have any plans to do so. Ryland is more of our marketing guru, so you may want to ask him about that.

### Is ferris chat gonna adopt any features guilded has and discord doesn't?
(from Littie6amer)

Personally I have no idea what's up with Guilded. I can't answer that personally.

### With the api permissions and different account types, will the authorization routes be limited to user accounts
(from Drx)

Probably. We'll see. You'll probably end up with a token for bots that you can regen from your user account at any time.

### Will the api return channels that the user doesn't have access to like Discord?
(from smallpepperz)

Nope. That was a bad move by Discord. We'll defo not show inacessible channels.

### Will you be able to use markdown in messages (if so, will it be like Discord markdown or full markdown)
(from smallpepperz)

Full markdown, just like what I'm typing here.

### Any plans on ui stuff such as "buttons" for the API?
(from jay3332)

Depends on how hard it is to implement in the client.


### If you get disconnected from the Websocket connection how will resuming work, or do we have to a full reconnect? And will there be sessions?
(from Drx)

Store session IDs.
Send resume with that ID.

### Will guild admins be able to give guild specific badges for their guild?
(from smallpepperz)

Nope.

### Will Ferrischat automatically create roles for bots joining with permissions? And will FerrisChat let you delete those roles without kicking the bot first?
(from smallpepperz)

Yes, and no.

### Would voice servers be seperate from the api?
(from Drx)

Yes.

### As a hypothetical, what if there is a server with some members, 20 of which create bots, then the server admins want to make a single bot role to simplify permissions. In this scenario, would there be a way other than kicking 20 bots and reinviting them with no permissions to give them only that central bot role? (Totally hypothetical)
(from smallpepperz)

There would be no other way. Also nice hypothetical.

### Will the "statuses" be the same as discord (online, idle, dnd, streaming, and mobile/web/desktop)
(from jay3332)

All but streaming, as that integrates with RPC.

### Will there be a pinned color option for roles that will make the user have that role color no matter if the roles above have a different colors?
(from Littie6amer)

I do like that idea, I'll see if I can implement it.

### Although sharding is optional how would we calculate what shard is entitled to which guild
(from Drx)

`guild_id % shard_count`

### Would their be sessions \[limits] like discord for the gateway?
(from Drx)

Yes, scaling with guild count.
Something like 1k at 100 guilds, increases 1 for every shard possible (500 to 750).

### Will FerrisChat be "playful" or "professional"?
(from jay3332)

Both, with a toggle to switch between the two.

### Is there gonna be a max member count for guilds?
(from Littie6amer)

Starting at something small like 1.5 to 2k to make sure the servers can handle it.

### Will accounts created currently on the PTA server exist when FerrisChat goes live?
(from smallpepperz)

Nope, as we'll most likely need to nuke the DB.

### Would verification exists for bots and how will it work
(from Drx)

Nope. The whole point is defeated by the fact it's easy to bypass.

### Will you ever be able to add what games you play to your profile?
(from Littie6amer)

Via about me, most likely.

### Will you have cool \[HTML] widgets and stuff?
(from smallpepperz)

Most lilkely not, as the user popout is native code, which isn't easy to translate to HTML.

### will there be custom emojis like on discord, and if so, will you be able to use them globally or just in the guild it was created in
(from Pumpkin)

Yes, globally.

### Embeds?
(from Cryptex)

Yes.

### Threads?
(from jay3332)

Depends.

### What will prevent user bots from being created?
(from smallpepperz)

Nothing except a lifetime ratelimit per IP.
Basically you can create up to (we're thinking) 40 accounts per residential IP and 4 per datacenter IP forever, never resetting.
You will also be able to report malicious accounts in-app, and if enough reports are gathered, you will get a public flag set on your profile.

### Are you using google sign in or handling login yourself?
(from Littie6amer)

We're doing native signin, plus some oauth2 signins, such as Facebook, Google, Twitter, GitHub, etc.

### Will FerrisChat's servers store deleted messages?
(from smallpepperz)

Nope.

### search message in a channel route
(from Cryptex)

Soon:tm:

### Will we allow to use Ferris oauth to sign in on our custom services like dashboards?
(from Drx)

Yes.

### Is data on ferris servers encrypted?
(from Littie6amer)

Not on the PTA, but on prod, we will do FDE.

### Search messages with regex?
(from Pumpkin)

Nope, as malicious expressions can freeze the server.

### Will there be statistics? Like messages per channel over time
(from smallpepperz)

I like the idea. It'll depend on how hard it is to implement.

### what are the plans on relationships (e.g. blocks, friends, pending friend requests)?
(from jay3332)

Blocks are a yes, friends are easy to add along with blocks.

### Could you fry an egg on ferris' servers when there is high load?
(from Littie6amer)

Ask our hoster.

### On a scale of 1-10, how many Easter eggs will there be?
(from smallpepperz)

6 to 8 on the professional mode, and 11 on the fun mode in app.

### How am I meant to pronounce 0/0?
(from smallpepperz)

* infinity
* NaN
* undefined

pick one

### will you ever be able to send attachment?
(from Cryptex)

Yup. It'll be uploaded to our CDN.

### Do you see any legal issues with having a CDN?
(from Littie6amer)

If we add it to our ToS that upload to our servers means we have the rights to distribute it, I see no problems.

### Will there be school hubs?
(from smallpepperz)

Nope. No point.

### Will you add server boosting?
(from Littie6amer)

Nope, that's paywalling stuff.

### Also will their be Stage channels?
(from Drx)

Yes.


