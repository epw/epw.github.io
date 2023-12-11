<link rel="stylesheet" href="/style.css">

# Logins: Simple, Secure, Private - Pick One If You're Lucky

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
  - Cons
    - They're notoriously weak and easy to break into
    - Hard to store them correctly and safely in your database
    - You have to set up a password reset flow if there are any users besides yourself
      - **Note:** Always think about the level of support you're getting yourself into. Will your friends and family be OK waiting for you to see a text in the morning when they couldn't remember their password last night? Do you need to set up an automated password reset flow, which means figuring out sending email that won't get blocked by spam filters?
- Emailed login links
  - Pros
    - You're relying on the security of their email account, which is likely pretty good
    - The email address makes an easy account identifier in your database
  - Cons
    - Sending automated email is rather difficult and often requires using third-party external systems to avoid spam filters of major email providers (Gmail, etc)
    - It can be pretty inconvenient to switch apps or devices just to log in, especially for people less familiar with technology
    - If it's possible any of your users don't have an email address, you'll need to build a whole secondary login system for them
- Texted login links
  - Pros
    - It's not a password
  - Cons
    - You're relying on the security of text messages, which is extremely bad by design.
    - You'll have to use an external system to send text messages
    - Some external systems that send text messages don't accept certain phone numbers, such as Google Voice numbers, which can cause unexpected and confusing errors
    - If any of your users don't have a phone plan with free texting, you'll need to build a whole secondary login system for them
