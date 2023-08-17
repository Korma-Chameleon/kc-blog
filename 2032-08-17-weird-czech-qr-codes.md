# Those weird Czech QR codes (2023-08-17)

One thing I noticed after moving to the Czech Republic is that all my bills
have QR codes on them. It also didn't take me long to figure out that my
phone banking app understands them.

In fact there is a whole QR payment system here. If I want someone to pay me
I do this:
1. Select "Send me money" in my banking app
2. Enter amount, message and some optional details
3. Share the QR code in Telegram, email, a letter or whatever.

The recipient pays me by:
1. Scanning the code in their banking app.
2. Scanning their fingerprint to confirm the transaction.

In effect, CZ has a payment request system that cuts out middlemen like PayPal
and works on any communication system from paper upwards.

So, how does it work?

The first step to figuring it out was to decode the QR code to some type of
data. Fortunately there are plenty of tools for this freely available. Here's
some examples I got:
```
SPD*1.0*ACC:CZ8320100000002800413427*CC:CZK*X-VS:732
SPD*1.0*ACC:CZ6520100000002000413429*CC:CZK*X-VS:799
SPD*1.0*ACC:CZ1355000000000000222885*AM:250.00*CC:CZK*MSG:FOND HUMANITY CCK*X-VS:333
```

Immediately we can see that it's text, it doesn't look encrypted and there is some
structure here; lots of stars and colons. Also, every example starrts the same way,
with `SPD*1.0*ACC:CZ`. That looks like something searchable!

After trying a few variations, I laded up on Wikipedia. Iy turns out that these are
an open standard named [Short Payment Descriptor](https://en.wikipedia.org/wiki/Short_Payment_Descriptor)
or just SPAYD. The format encodes a list of key-value text fields representing
payment information and the structure is very simple, using only a couple of separators.

Armed with this information, I figured this would be a good project to try in
Rust. I'd played around with the language a little before and heard about a
parsing library called [Nom](https://crates.io/crates/nom) which seemed like it
would fit here. After a couple of hours, I had put together some basic data
structures for storing the text fields and a parser that seemed to work. With
some enhancements, I expect that I can turn this into a usable library. And if not
it'll be good for learning.

My initial version is [now available](https://crates.io/crates/spayd).
