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

For me (hydro), the chain events that is outlined in [Danny's Gist](https://gist.github.com/Rapptz/4a2f62751b9600a31a0d3c78100287f1) made me want something better than Discord. The shitty, rude, and almost incoherent decisions by Discord's staff members made it seem like it was no longer a community building software run by a small company who listens to their users. It started to seem more like a corporation with some sort of monetary goal (proved by their pushing of nitro).

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
