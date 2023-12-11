<link rel="stylesheet" href="/style.css">

# A Personal Server is All About Tradeoffs

Let's say you have a cool idea, and all you need for it is to be able
to enter information on your phone and do things with it later. It
would probably be nice if when you upgrade phones you don't lose
everything, and maybe eventually you'd like to be able to allow a
second person to use it, too.

Unfortunately, this probably means you need a "server", which in this
case means a program that is always running and can collect the data.

The first time you do this, it can be pretty frustrating and involve
choosing among a lot of bad-seeming options. But, once you have
a setup you like, it quickly becomes something you can take for
granted that enables whole new types of projects.

Here are your main options:

- Mobile app only
  - Pros:
    - Everything under your own roof
    - No (or minimal) added security vulnerabilities
    - Minimal cost if you already have a smartphone
  - Cons:
    - Most technologically difficult implementation
    - Connectivity issues are likely
    - You're stuck with the one device, unless you do a lot of extra work
    - Little can be re-used if you later want to change to any other method
- Web app server running in your home
  - Pros:
    - Everything under your own roof
    - Large number of options
    - Low cost (you might be able to re-use an existing computer, or
      buy a $15 one)
    - Much of the system will be reusable if you want to change to
      external hosting
  - Cons:
    - A computer always has to be running
    - The computer will be reachable from the Internet, which is a
      larger security vulnerability than normal computer usage involves
    - Sometimes technically against your ISP's terms of service (they
      won't notice or care but you might have to slightly work around
      it)
    - You'll almost certainly have to rent a domain (at least
      $12/year) and configure Dynamic DNS and port forwarding
- Web app server running on external hosting
  - Pros:
    - No security implications for your own home
    - Best documentation how to actually start a website
    - Often has a (long, inconvenient) domain name for free
  - Cons:
    - Most expensive (at least $5/month, likely more)
    - Not under your own roof
- "Serverless"/Lambda/Cloud Functions
  - Pros:
    - No security implications for your own home, few security
      considerations for the system you build
    - Minimal cost (there are free tiers you will almost certainly fit
      into)
    - Usually good documentation
    - Often has a (long, inconvenient) domain name for free
  - Cons:
    - Most locked into one company's system
    - Unlike other server configuration so harder to move to external
      hosting or a server running in your own house
    - Very limited in the types of programs you can run

For most people, I recommend the web app server running in your own
home if ownership outweighs the security risks for you, "Serverless"
if you just need a website with maybe a small database or similar data
storage, and a Web app server running on external hosting otherwise.

And, if you really hate all of them, there's a sub-option "Web app
server running in your own home that is not accessible from the wider
Internet," which replaces the security vulnerability con with
"connectivity issues are likely."

Once you finally have your server worked out, you'll probably notice
pretty quickly that you want to have some sort of authentication or
user account system, so that not just anybody can poke your server,
and you can do things like identify users so you don't conflate their
actions. [Unfortunately, login systems are an even bigger
hassle.](logins.html)
