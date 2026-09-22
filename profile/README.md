# pitblu

**Open-source software for making BBQ hardware work for you.**

I love BBQ.

Not just throwing a few burgers on a grill, but the whole thing: smoking meat for hours, managing temperatures, experimenting with different cookers, watching a brisket slowly come together, trying something new, getting it wrong occasionally, and generally spending far too much time thinking about fire, smoke and meat.

I use a Weber kettle, a Weber Smokey Mountain and a Weber iGrill 2 with multiple temperature probes. The iGrill hardware is actually pretty good.

The problem was the software.

It only really wanted to live on my phone over Bluetooth. I wanted to see the temperatures on my PC. I wanted Wi-Fi. I wanted multiple dashboards. I wanted to be able to use the data somewhere else, connect it to other things and generally do more with a perfectly good piece of hardware I already owned.

pitblu started as a way of getting more out of my iGrill, but it has grown into something broader: an open platform for getting temperature data out of BBQ hardware and then doing whatever you want with it.

If I want to send an alert, hook it into some automation, build another application around it or do something completely ridiculous that nobody else would ever want to do, I should be able to.

That's really what pitblu is about.

I also wasn't starting completely from scratch.  

One of the great things about open source is discovering that other people have already looked at a piece of hardware and thought, "I wonder what else I can make this do?"

A number of iGrill projects helped me understand what was possible, particularly bendikwa/esphome-igrill, alongside jaydenk/igrill-remote-server, pilot1981/weber-igrill-integration-HA, 1mckenna/esp32_iGrill and sanjay900/igrill.

pitblu isn't a fork of those projects and doesn't copy their implementations. They were research, reference and inspiration: people who had already done the hard work of poking at the iGrill protocol, figuring out how it behaves and sharing what they'd learned.

Without that work, pitblu would have been a much harder project to start.

And I love that. Someone works something out, shares it, somebody else learns from it and builds something different. That feels very much in the spirit of what pitblu is supposed to be.

## The Octoblu spirit

There's another reason pitblu ended up being built this way.

**Octoblu.**

Octoblu was incredibly important to me.

I'm not a developer, and I've never pretended to be one. What Octoblu did brilliantly was allow someone like me to take different things, connect them together and make them do something useful.

That was magic.

It made technology feel open.

You didn't have to accept that device A lived in one little world and service B lived somewhere else. If you could connect them, you could make them work together.

And if you had an idea, you could have a go at building it.

The philosophy I took away from Octoblu was really simple:

> **There should be no restriction on connecting things together so that they work for you.**

I loved the technology, but honestly, the people were every bit as important.

I was lucky enough to spend time with **Chris Matthieu and the Octoblu team**. I even paid my own way out to Arizona a couple of times because I wanted to work with them, learn from them and just be around what they were building.

They taught me loads.

More importantly, I made some lifelong friends.

It was an amazing time in my life and one I still look back on with huge fondness.

So years later, when I'm standing next to a Weber Smokey Mountain thinking about why my perfectly good thermometer can only really talk to one app over Bluetooth, some of that Octoblu thinking inevitably kicks back in.

Why should it only do that?

Why can't the data go over Wi-Fi?

Why can't I see it on a PC?

Why can't I build another dashboard?

Why can't I connect it to something else?

And, ultimately:

**Why should I replace something that works when I can make it work better for me?**

That's the spirit behind pitblu.

pitblu isn't trying to recreate Octoblu. Not remotely.

It's a little BBQ project solving a very specific problem.

But I hope a bit of that same spirit runs through it:

**connect the things, expose what they can do, remove unnecessary restrictions, and let people decide what they want to build.**

## Project architecture

pitblu separates hardware communication from the applications that use it.

There are currently two primary projects.

### [pitblu-core](https://github.com/pitblu-project/pitblu-core)

The hardware and telemetry layer.

`pitblu-core` communicates directly with supported Bluetooth thermometer hardware and exposes its capabilities through a local API.

Responsibilities include:

- Bluetooth Low Energy device discovery
- iGrill device registration and connection
- Physical probe detection
- Temperature telemetry
- Device battery information
- Connection and reconnection management
- REST API
- Server-Sent Events for live telemetry
- Hardware abstraction for future device support

`pitblu-core` is deliberately independent of the pitblu user interface.

Anything capable of consuming its API should be able to use it.

---

### [pitblu-app](https://github.com/pitblu-project/pitblu-app)

The user-facing cooking application.

`pitblu-app` consumes the services exposed by `pitblu-core` and turns probe telemetry into a practical BBQ monitoring experience.

The application is designed around the **cook**, rather than around the thermometer.

Current and planned capabilities include:

- Live probe temperatures
- Multiple thermometers and probes
- Cooker and food temperature monitoring
- Cook setup workflow
- Probe assignment and reassignment during a cook
- Target temperatures and temperature ranges
- Approaching-target alerts
- Live cook dashboard
- Temperature charts
- Cook history and journal
- Read-only shared cook displays
- QR-code sharing
- Progressive Web App support
- REST and SSE APIs for integrations

The frontend is built with **React and TypeScript**, with **FastAPI** providing the application API and serving the production frontend.

## How it fits together

```text id="dn58zk"
┌───────────────────────┐
│ Bluetooth Thermometer │
│     Weber iGrill      │
└───────────┬───────────┘
            │ BLE
            ▼
┌───────────────────────┐
│      pitblu-core      │
│                       │
│ Hardware + Telemetry  │
│ REST API / SSE        │
└───────────┬───────────┘
            │
            │ API
            ▼
┌───────────────────────┐
│      pitblu-app       │
│                       │
│ Cook Management       │
│ Monitoring + UX       │
└───────────┬───────────┘
            │
            ├──────────────► Web / PWA
            │
            ├──────────────► Shared Cook Display
            │
            ├──────────────► Home Automation
            │
            └──────────────► Third-party Integrations
```


## Project status

pitblu is under active development.

The current focus is establishing the core architecture and primary cooking workflow:

```text id="r0tiz5"
Discover thermometer
        ↓
Connect probes
        ↓
Start cook
        ↓
Tell each probe what it is monitoring
        ↓
Monitor the cook
        ↓
Finish cook
        ↓
Review cook history
```

Support currently focuses on the **Weber iGrill 2**, while the architecture is intended to support additional thermometer hardware in the future.

The intention isn't to build one enormous BBQ application.

It's to build a small collection of well-defined components that can work together, and that other people can build on.

---

**Connect the things. Own the data. Make them work for you.**
