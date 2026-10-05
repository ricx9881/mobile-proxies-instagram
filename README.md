# Rotating US residential, HTTP
curl -x "http://login__cr.us:password@gw.dataimpulse.com:823" https://api.ipify.org/

# Sticky session, 30-minute default, same IP across requests
curl -x "http://login:password@gw.dataimpulse.com:10000" https://api.ipify.org/


There's also a `sessid` parameter for pinning a specific labelled IP for roughly 30 minutes — handy when you want stable identity without occupying a sticky port, or when you need to return to an IP you used earlier. If the underlying device drops offline, the system silently reassigns you to another available one, so don't build anything that assumes a fixed address forever.

The network behind the endpoint is listed at 90M+ ethically sourced IPs across 195 countries [4][6], with a published success rate of 99.51% and a 4.8/5 rating on G2 [6]. TechRadar's hands-on review reports residential proxies delivering a consistently high scraping success rate and singles out non-expiring traffic as the platform's clearest differentiator from subscription-based competitors [7]. Treat vendor-published success rates as a starting assumption to test, not a guarantee — the only number that matters for your budget is your own success rate against your own targets.

## What backconnect residential traffic costs

This is where the pay-as-you-go model diverges sharply from the monthly-bundle model most "backconnect proxy" pages still assume. You fund a balance, traffic is deducted as you use it, and leftover volume doesn't reset.

| Plan | Core specs | Entry price | Billing model | Purchase |
| --- | --- | --- | --- | --- |
| **Residential** | 90M+ IPs, 195 countries, rotating + sticky, HTTP(S)/SOCKS5, free country targeting | **$5 for 5GB** ($1/GB) | Pay-as-you-go, no subscription, traffic never expires | [Start with the $5 residential intro plan](https://bit.ly/dataimPulse) |
| **Premium Residential** | High-speed residential pool, all targeting options included at no surcharge, dedicated account manager | **$5 for 1GB** ($5/GB) | Pay-as-you-go, no subscription, traffic never expires | [See the premium residential option](https://bit.ly/dataimPulse) |
| **Mobile** | 4G/5G/3G/LTE carrier IPs, hardest targets | **$5 for 2.5GB** ($2/GB) | Pay-as-you-go, no subscription, traffic never expires | [Check mobile proxy pricing](https://bit.ly/dataimPulse) |
| **Datacenter** | 99.9% uptime, random subnet access, cheapest tier | **$5 for 10GB** ($0.50/GB) | Pay-as-you-go, no subscription, traffic never expires | [Compare datacenter plans](https://bit.ly/dataimPulse) |

Volume discounts kick in at higher tiers, and the residential and mobile rates drop 20% at 1TB [5][6]:

- **Residential:** 50GB for $50 · 100GB for $100 · 1TB for $800 ($0.80/GB)
- **Premium Residential:** 10GB for $50 · custom pricing from $20,000 for 5TB+
- **Mobile:** 25GB for $50 · 1TB for $1,600 ($1.60/GB) · custom pricing from $8,000 for 5TB+
- **Datacenter:** 100GB for $50 · 1TB for $450 ($0.45/GB) · custom pricing from $2,250 for 5TB+

For context on whether $1/GB is actually cheap: the 2026 market range for residential traffic runs from about $1/GB at the value end to $5–8/GB at enterprise tier, with the industry average sitting around $3–8/GB [6][8]. Several independent comparisons put DataImpulse at the floor of that range specifically because of the no-expiry, no-subscription structure — Webscraping.ai's cost analysis concludes that under roughly 50GB a month, flat $1/GB with a $5 minimum beats bundle subscriptions, because subscriptions charge you for volume you don't finish [8].

That last point matters more for backconnect workloads than for almost anything else. Scraping traffic is lumpy. You burn 40GB in a launch week and 2GB the next month. A monthly bundle resets; a balance doesn't.

## Which proxy type your job actually needs

The temptation is to buy residential for everything, because "backconnect residential proxies" is what you searched for and residential is what the term implies. That's often the expensive choice.

Rotate by target instead:

- **Residential ($1/GB)** for defended pages — search results, e-commerce listings with anti-bot layers, social platforms, price and review monitoring [6][9].
- **Datacenter ($0.50/GB)** for your own infrastructure, public reference pages, and targets that don't inspect IP type. You get double the traffic for the same $5, and advanced targeting reportedly comes included rather than surcharged.
- **Mobile ($2/GB)** only when residential fails — mobile app data, carrier-specific content, the handful of targets that hard-block non-cellular ranges.
- **Premium Residential ($5/GB)** when a run is business-critical and you want the faster pool plus a named account manager. All targeting options are included at no extra charge, which partially offsets the 5× rate if your work is city- or ZIP-heavy.

That city-level detail is the one billing trap worth internalizing. Country targeting is free on residential. State, city, ZIP, and ASN selection is billed at **double the standard per-GB rate** on standard residential plans [6]. A job that needs ZIP-level US targeting effectively costs $2/GB, not $1 — still below the market average, but not the headline figure. DataImpulse's documentation points this out directly, and if your budget depends on it, confirm the current treatment with support before you commit.

## Setting up a backconnect endpoint end to end

The setup path is short enough to describe without hand-waving:

1. Create an account and add a plan. You select the proxy type in the dashboard and top up a balance — there's no subscription to cancel later.
2. Build your endpoint from the gateway host, your credentials, and the port that matches the connection type: 823 for HTTP/HTTPS, 824 for SOCKS5, 10000–20000 for sticky.
3. Append country codes to the username (`__cr.us`, `__cr.de`, and so on) instead of reconfiguring a dashboard setting per target.
4. Verify before scaling. A single curl against an IP-echo service tells you whether rotation is working and which country you landed in; run it ten times and you should see ten different IPs.
5. Integrate. HTTP(S) and SOCKS5 work with Scrapy, Selenium, Puppeteer, and most proxy managers without custom code, which is why "backconnect" setups are usually a config change rather than a development project.

## Limitations to weigh before you top up

A few things the marketing pages won't lead with:

- **No free trial.** Access starts at a $5 minimum purchase. Intro plans carry a 7-day money-back guarantee for card payments, conditional on using less than 80% of the traffic — crypto purchases on Intro plans aren't refundable [5]. Five dollars is a cheap test, but it isn't free, and it isn't unconditionally refundable.
- **Rotation isn't a fingerprint solution.** Backconnect proxies change your IP. They don't change your user agent, canvas fingerprint, or request timing. If a target segments on browser behaviour, IP rotation alone won't carry you.
- **Sticky means "usually sticky."** One to 120 minutes advertised, around 30 typical, with early rotation when the host device disconnects [5]. Design retries accordingly.
- **Not a static ISP product.** If you need a dedicated, unchanging IP for account management or whitelisting, rotating residential is the wrong category. DataImpulse's own positioning is rotating residential, mobile, and datacenter traffic for public data collection.
- **Not for banking or government portals.** The provider's own guidance steers users away from those targets.

## Quick answers

**Is a backconnect proxy the same as a rotating proxy?** Functionally yes, when the pool is residential [3]. "Backconnect" describes routing through one endpoint; "rotating" describes changing the IP per request or on an interval.

**Do I need a separate backconnect plan?** No. You configure an endpoint against a residential gateway and rotation is handled for you.

**How much does residential backconnect traffic cost?** Around $1/GB at the value end of the 2026 market, up to $5–8/GB at enterprise tier. Double it if you need city, ZIP, or ASN targeting on a standard residential plan.

**What's the minimum spend?** $5, which buys 5GB of residential, 10GB of datacenter, 2.5GB of mobile, or 1GB of premium residential.

**Does unused traffic expire?** Not on pay-as-you-go plans that state non-expiry. Confirm this per provider — expired GB is the most common hidden markup in proxy pricing [6][8].

## Bottom line

The word "backconnect" describes a gateway pattern, not a product. Once you strip it down, the buying decision is about pool quality, rotation control, and whether you're charged for volume you never finish.

👉 [Open a DataImpulse account and test the $5 residential intro](https://bit.ly/dataimPulse)

For uneven workloads — a burst of scraping here, quiet weeks there — a $1/GB balance that never expires removes the waste built into monthly bundles. Run your own target list through it for a few gigabytes, measure cost per successful request rather than cost per GB, and scale only if that number holds up. If it doesn't, you've spent five dollars finding out, which is a better outcome than discovering it three months into a subscription.
