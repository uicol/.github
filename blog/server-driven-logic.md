# Server-driven Logic

Mobile apps can be split into two categories roughly: offline and online. Of course it's a continuum and not a binary separation, like a game could be local but sync some data to the server. But helicopter view allows us to have a split like that.

I'm interested in the second category, since I've been a CTO of [kasta.ua](https://kasta.ua) — a big player in Ukrainian e-commerce — for a long time. And e-commerce couldn't be done locally: stock is changing all the time, prices and descriptions too, new products are coming often, old products are being retired/changed/etc... If you think I haven't thought about offloading whole product DB to a phone and then going to server only for updates and checkout then think twice. 😁

When we started a global makeover of the entire platform (back in 2016), I designed an API returning honest data — whatever database knows. For example, an order has those products having this price, this is how many bonuses were used, this is how much money was already paid (since we have both pre-payment and post-payment).

And a business logic of displaying that data to the client we did on the front-end: i.e. how much money you'd have to pay when receiving an order. Of course, it was done in the web client first, where development of an API and a client was tightly connected. It was often done by the same developer within one task.

After that apps usually got a task saying "_just_ do what the web does". And the _just_ did the same logic. Developers even learned how to read Clojure. 😁 But people are people, and sometimes were was a mistake, sometimes different decision was being made, sometimes something was missing... And apps behavior was different from the web and from one another. 🤣

Obviously that was highly irritating! So a few years ago we decided that there should be no calculations or decisions made on client. If you need to show how much should be paid for an order this is never being done by an app. You can't just sum all products prices and subtract the payment — someone will forget about bonuses, someone will forget about amount of products. Every number displayed for the customer should be returned by an API call. Even if there is a mistake, it will be the same for everyone and it'll take just one fix to solve it.

Checkout is the same. We had an API which expected you to send complete order data, and client had to go through different API calls to gather delivery methods, payment methods, decide where they are applicable — last one was a major pain! Logic was complex and it did change with time. Like one of the apps allows you to select bonuses which are not applicable for the order — just because bonus API has no idea if you're making an order or looking at your bonus listing page.

So it was reworked into something like wizard, where you go through step by step and API returns data what should be asked next, what's enabled, what's disabled (with a tip why is it disabled!). You get delivery methods through Checkout API, available payment methods — through Checkout API. Business logic should be on the server.

Summary: when your app is not a video editor and not a game, your clients (apps and web) are all about markup, ergonomics and display. It's not about calculation and being smart. Since today you are smart and cunning, but tomorrow you have a bug. 😁
