# Q&A Questions and Answers

### Would they be any type of system once a user hit a ratelimit on a endpoint to much times that it turns global
(from Drx)

Currently there are no plans to do so, as we don't have global ratelimits in the server.
This may change in the future (mainly depending on how ~~poorly~~ nicely you all use the API), but you'd still need a *lot* of requests to hit this (most likely somewhere in the range of 2,500 to 7,500 429s in a 10 minute period).

### What parts of FerrisChat will be similar to Discord, and what parts will be significantly different?
(from smallpepperz)

We're trying to keep it somewhat similar to not confuse those coming from Discord, but there are already a few big changes:
* snowflakes: unsinged 128 bit ints, unlike Discord's unsinged 64 bit
* a lot of API routes are similar to Discord, but a lot of them are also completely different
* and of course, a dev team that actually listens to people and doesn't screw them over with random changes

### What are some benefits that donaters will have?
(from Ryland)

Probably nothing. Except a badge. If (and only if) we absolutely need to, we'll only add new features (not big features, only small ones) that require donating.

### When do you guys think this project will be finished?
(from Chasee)

No comment. We don't want to give a improper estimate and then disappoint people because it wasn't correct.

### What was the motive behind ferris chat? How will it be different from other platforms?
(from MrKomodoDragon)

For me (0/0) it was based off the fact Electron is... horrible at first, and later on when Discord introduced the message intent, I got fed up enough and started working on it.
If hydro feels like he should add something here, go ahead.

### Will there be any ratelimit bucket header?
(from Cryptex)

Yup. Can't give you a name *yet* but there'll be two:
* one specifying total requests
* one specifying number of seconds for those total requests

The body of 429 already has a JSON response if I remember correctly that wraps these two up and adds a `retry_after` field.

### What will the source of emoji be? (Eg Discord uses Twemoji)
(from jay3332)

Probably also Twemoji. Thanks to Izmoqwy's digging, we did find out that it might not be so easy to render it, as the built-in text renderer in `iced` makes you pick one font for a widget.

### Will FerrisChat be centralized (like Discord, Slack, Facebook, etc) or decentralized (IRC, Matrix, etc)?
(from Sophon)

Centralized, but if you so desired (later on, right now a lot of things are hardcoded) it'd be easy to set up your own instance. You wouldn't be able to easily set up your own clients tho, since a lot of them are probably hardcoded with the main API.

### Will there be an official client?
(from Sophon)

Yup. Also Rusty. Using `iced` and, depending on how hard it is to implement, allowing you to switch remote servers from the official servers to any selfhosted variant.

### Will there be a mobile app in the App Store for FerrisChat?
(from Al3amode)

valkyrie_pilot is currently working on one. Send condolences to them for needing to deal with Swift.

### How will permissions work?
(from hydro)

Pretty similar to Discord. You get a set of bitflags (made with Rust's `bitflags` crate, fwiw), and each bit corresponds to a permission the user has.
If you hit a endpoint that needs permissions, server goes to Redis hoping your permissions for that channel/guild are cached, otherwise falls back to DB.
If it turns out you don't have perms, you get hit with good old 403 Forbidden.
When a user changes permissions, it'll be instantly removed from Redis, leaving the next call to fall back.
