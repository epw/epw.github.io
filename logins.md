<link rel="stylesheet" href="/style.css">

# Logins: Simple, Secure, Private - Pick One (If You're Lucky)

Say you finally have a [server](servers.html) for a little personal project. Maybe you can push a button on a webpage or app and the push will be recorded. It works so well that you want to share it with one other person.

What do you do?

We're all familiar with login systems. There are usernames and passwords, sometimes auto-filled by a password manager. There are "email me a magic link" buttons. There are "Log in with Google/Facebook/GitHub" buttons. There are even secret links that you just have to hope nobody shares in the wrong place.

If you are exposing a database to the Internet (which you have to, if you want to be able to use the little tool you built from anywhere), you have a series of bad options. If you want to let additional people also interact with that database, your options get worse.

You might want to rely on "security through obscurity": Nobody knows about your little website, so they won't direct large amounts of effort into breaking in, right? Unfortunately, there are enough automated worms and viruses out there that you might get hit by an attack anyway, and in the best case all that will happen is a lot of junk data will be written into your system.

Here are all the possibilities I know of.

- Passwords
  - Pros
    - Everybody is familiar with them
    - Password managers are pretty good and can sync them across devices
    - Everyone who can reach your website has what they need to use a username and password
    - Flexibility in how you implement it, from a normal custom login page to HTTP Basic Authentication (browser popups)
    - Easy to make an automated "register a new account", or to manually create accounts when asked
  - Cons
    - They're notoriously weak and easy to break into
    - Hard to store them correctly and safely in your database
    - You have to set up a password reset flow if there are any users besides yourself
      - **Note:** Always think about the level of support you're getting yourself into. Will your friends and family be OK waiting for you to see a text in the morning when they couldn't remember their password last night? Do you need to set up an automated password reset flow, which means figuring out sending email that won't get blocked by spam filters?
- Emailed login links
  - Pros
    - There's no password to leak even by accident
    - You're relying on the security of their email account, which is likely pretty good
    - The email address makes an easy account identifier in your database
    - Easy to make an automated "register a new account", or to manually create accounts when asked
  - Cons
    - Sending automated email is rather difficult and often requires using third-party external systems to avoid spam filters of major email providers (Gmail, etc)
    - It can be pretty inconvenient to switch apps or devices just to log in, especially for people less familiar with technology
    - If it's possible any of your users don't have an email address, you'll need to build a whole secondary login system for them
    - Can be used to spam someone if you aren't careful in your implementation
    - Either it's too easy for someone illicitly in the account to change the email address and steal the account, or it's too hard for someone who has lost access to their email account to get back into their account on your system.
- Texted login links
  - Pros
    - There's no password to leak even by accident
    - The phone number makes an easy account identifier in your database (though phone numbers change more often than email addresses)
    - Easy to make an automated "register a new account", or to manually create accounts when asked
  - Cons
    - You're relying on the security of text messages, which is extremely bad by design.
    - You'll have to use an external system to send text messages
    - Some external systems that send text messages don't accept certain phone numbers, such as Google Voice numbers, which can cause unexpected and confusing errors
    - If any of your users don't have a phone plan with free texting, you'll need to build a whole secondary login system for them
    - Can be used to spam someone if you aren't careful in your implementation
    - Either it's too easy for someone illicitly in the account to change the phone number and steal the account, or it's too hard for someone who has lost access to their phone number to get back into their account on your system.
- Log In With &lt;Google/Facebook/GitHub/etc&gt; button (formally called "OAuth2")
  - Pros
    - Very good security
    - If your user leaks their password it's not your problem
    - Largely well-understood and recognized
    - Certain systems (like Google App Engine) have libraries that make implementing this simple
    - Easy to automatically create accounts
  - Cons
    - Unbelievably hard to implement if you aren't using a system with a very, very good built-in library
    - Also hard to implement if you aren't using a language that the company in question hasn't provided a library for
    - The major implementations have changed every 5 years or so for a while, so may not be stable
    - Some amount of information on your users is potentially logged by the company or companies you use as a button provider
    - Users have to have an account in at least one of the companies you are using as a button provider
    - A little tricky to stop unwanted users from creating accounts
    - Either it's too easy for someone illicitly in the account to change the phone number and steal the account, or it's too hard for someone who has lost access to their phone number to get back into their account on your system.
- Secret links
  - _This is the concept where your user doesn't go to `example.com`, they go to `example.com/private?key=a-really-long-string-of-letters-and-numbers-here`_
  - Pros
    - Simple to implement
    - Convenient for users: they can bookmark the secret link, save it in a document, copy it to their different devices, or whatever else they want
  - Cons
    - No real security: if the link is ever posted in the wrong place, say by a copy/paste error, it gives instant access
      - It's hard to overstate how weak this really is
    - You have to figure out how you want to generate and provide new secret links for new "accounts", there won't be much guidance
    - A bit inconvenient to do in mobile apps rather than websites
    - Doesn't work very well on shared browsers
- HTTPS Client Certificates
  - Pros
    - Uses a (little known) browser standard
    - Uses public-key cryptography for authentication, which is extremely secure and can have additional security layered on top
    - There's no password to leak even by accident
  - Cons
    - Takes extensive Web server configuration, which won't even be possible on simpler systems
    - Poorly documented
    - Requires confusing interface with the user's operating system, and the UI varies dramatically by operating system
    - Has inconvenient timeouts (may require user to "log in" every time they visit, depending on device)
    - No good automated account-recovery system exists
    - Each device will appear unique, and you have to write the system to link a given user's devices together into one "account" yourself
    - Hard to make new accounts, will require plenty of custom code
    - Difficult or impossible to use in mobile apps rather than websites
    - Shared browsers will require that each user pick a passphrase, which is inconvenient and normally unnecessary, otherwise
    - Users won't be used to this and won't find it natural
- Wireguard
  - _To use Wireguard, you install and configure it on your server, then install and configure a VPN-like client on all the devices that you want to have to access it. The VPN has to be turned on to reach your server._
  - Pros
    - This lets you write your login system as if everyone were on your LAN/WiFi and you can trust the devices.
    - Wireguard's security model uses public-key cryptography for authentication, which is extremely secure and can have additional security layered on top
  - Cons
    - Pretty tricky to set up; Wireguard aims to be as easy as SSH keys, but they aren't there yet, and SSH keys aren't the simplest system in the first place
    - You still have to do some level of account management, such as linking devices together for the same user
    - Hard to make new accounts, you'll have to do configuration on the server and the client
    - Users won't be used to this and won't find it natural
- Tailscale
  - _I don't actually know much about this, they're a company based on Wireguard and so supposedly make it much easier to use. They have a free tier for home users._
- Authenticator app codes
  - _This refers to using an app like Google Authenticator that shows a code for a period of time, usually 6 numeric digits for 10-30 seconds before refreshing. It's usually used as the second factor on usernames with passwords, but nothing stops you from just treating the code as someone proving they once had access to the account in question, and so if they provide the right code they can log in._
  - Pros
    - Fairly simple to implement in server (as easy as you might expect passwords to be; and so easier than passwords actually are)
    - Users who are used to authentication apps will find it easy to use as a login system
    - Security is pretty good: the correct code being presented is a pretty good sign that the same phone is being used as was used to create the account, and leaked codes quickly become useless, unlike leaked passwords
    - Easy enough to have an automated account creation flow or to hand out accounts, assuming you can display images like QR codes
  - Cons
    - Not a lot of documentation exists for this yet
    - User has to switch apps or devices to get the code, and always has to have a device with the relevant secret loaded handy (frequently, their phone)
    - Many users are unfamiliar with authenticator apps
    - Some users struggle copying authenticator app codes from the authenticator app to the login page in time
    - Users won't be used to this and won't find it natural
- Browser storage keys
  - _This refers to saving public and private keys in the user's browser (in something persistent like localStorage or IndexedDB), and using them to log the browser in as requested. The draft [RFC 7486](https://datatracker.ietf.org/doc/html/rfc7486) was an earlier description of a similar idea._
  - Pros
    - Uses public-key cryptography for authentication, which is extremely secure
    - There's no password to leak even by accident
  - Cons
    - I haven't finished the [library](https://github.com/epw/hoba-tools) that would make this easy
    - TLS security is extremely hard to get right when written by hand, which is required when there isn't a functioning library
    - Logging into a new device usually requires a device with a working login for the user, or else requires something like the email, text, or secret link options above.
    - Users won't be used to this and won't find it natural
- Passkeys/FIDO/WebAuthn/Web Authentication
  - _This is the hot new thing that involves physical USB or NFC "security keys" or sometimes a phone or fingerprint reader._
  - _I don't understand this yet and have found it extremely hard to use as an individual developer._

You could also add two-factor authentication to any of these, most likely passwords, but you'll have to figure out how you deliver the two-factor codes, which usually is equivalent to the email, text, or authenticator app options, and so adds all of their cons, in addition to the added inconvenience to the user of needing to switch apps or devices.

Once you have finally made some decisions, you will still have a myriad of other considerations, such as:
- How do you want to handle logging out? Will users be logged out automatically by cookie expiration?
- How will you make sure your code actually implements user data siloing and won't accidentally read or write from the wrong user?
- Are you using a session cookie? Where is it stored? If not, how else are you doing this?
- What happens if the user clears their cookies?
- How will you protect against XSRF attacks from unauthenticated users?
- Do you need to worry about cookie hijacking? (probably not if you're using HTTPS and don't allow users to run untrusted JavaScript on the page, but you never know)

My general advice:
- If you are already using a cloud-based system such as AWS or Google's Firebase, pick an option with good OAuth2 support ("Log In With..." buttons) and use that
- If you are writing a tiny system with, say, 5 or fewer users who all know each other and the data isn't very sensitive, then use secret links and make frequent backups (in particular, back up onto a system that is separate from the web server serving the secret links)
- Otherwise, I'd recommend the authenticator app code system, even though it isn't used very commonly yet. It gets you much of the ease of usernames and passwords, without a number of the hidden gotchas.
- If you end up writing your own username/password system, at least now you have more of a sense of why even though that's bad, all the other options are too.

### Closing thoughts

It's conceivable that one day, there could be a library that is relatively simple to drop into a website, with a few files implementing a login page, secure (likely key-based) account management system, and a simple file-based database, so that the rest of the site can just call a few library functions to query the database and check that a given Web request comes from a logged-in user. At the time of writing (December 2023), the closest things are probably OAuth2 libraries, which the author clearly dislikes after years of struggling with their complexity. Hopefully one or more projects will be completed so that by the end of the decade, aspiring Web developers will have a few good options with clear tradeoffs.
