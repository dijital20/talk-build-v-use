# Build vs Use

In any project of a certain size, you will come across a point where you need a feature that isn't provided by the language or standard library. At that point, you will have a choice whether to install some-- hey, stop installing `requests` and listen for a minute! As I was saying, you will get to a point where you have to choose whether to write the functionality yourself or pull in some hopefully brilliant, well-maintained, secure bit of third party code that does the job for you. Take a moment to imagine being at the crossroads, weighing your options, and let's mull out the details.

Both paths are full of wonders and pitfalls; and let me cut to the chase, there's no singular "right" solution here. Sometimes one path is the right one, sometimes the other; and which is right depends on a number of different factors, both for yourself, and for your potential users (assuming they aren't you).

I have an idea... anyone here seen the movie Sliding Doors? In that movie, Gwyneth Paltrow's character lives two different lives depending on whether she makes it to her train or misses it. Let's use that same idea to explore what happens when you build your own vs use someone else's.

### What if we decided to build our own solution?

You're building a framework to validate and serialize/deserialize JSON requests and responses. The decision is made—you're going to build it yourself.

#### Speed of Delivery

Day one feels great. You start coding. No dependency to hunt down, no documentation to read, no learning curve for someone else's API. You know exactly what you need: validate incoming JSON against a schema, transform it into Python objects, serialize responses back out. You're writing code by lunchtime. Your first endpoint works by end of week. You feel productive.

#### Control & Fit

This is where the build path reveals its primary advantage. Your validation framework fits _your_ codebase like a glove. Your error messages match your conventions. Your serialization respects your naming patterns. When you need to add a custom validator for your domain—say, a specific way of handling timestamps that's unique to your business—you add it directly. No fighting with library conventions. No working around limitations. No "I wish it did X" moments. It does exactly what you need, shaped exactly how you want it.

Six months in, you've integrated it across dozens of endpoints. Your team knows it intimately because you built it together.

#### Documentation and Support

But now your teammates are asking questions. "How do I add a custom validator?" "Why does this error message say that?" "What's the difference between `serialize_mode` and `strict_mode`?" You have to answer them. You have to document your framework—not for external users, maybe, but for your own team. You write docstrings. You create examples. You build a wiki page or internal documentation.

Then someone joins the team who wasn't there when you built it. They're confused. They need onboarding. You spend time explaining design decisions that seemed obvious to you but aren't documented anywhere. You realize you need better documentation. You spend evenings writing it.

If your framework becomes widely used across your organization, you become the de facto support person. Someone's validator isn't working the way they expect? They ask you. Someone wants a feature? They come to you. You're not just maintaining code; you're maintaining people's understanding of your code. You're the documentation, the FAQ, the Stack Overflow answer all rolled into one.

#### Bug Fixes

Then a user reports something strange: deeply nested objects with null values aren't serializing correctly. You investigate. It's your code. You own this completely. No waiting for a maintainer's response. No hoping they prioritize your issue. You debug it, fix it, deploy it on your schedule. This is the other side of control—when something breaks, you're responsible for the fix. You spend a Tuesday afternoon tracking down an edge case you didn't anticipate. It's yours to own.

#### Security Updates

Three months later, someone on your team mentions a potential vulnerability in how you're handling untrusted JSON input. Maybe there's a denial-of-service vector through deeply nested payloads. Maybe there's a way to exploit your type coercion. You have to think about these things. You have to stay vigilant. You have to monitor security disclosures related to JSON handling, Python's serialization, and your own implementation choices. When you identify a risk, you patch it. When you deploy it, your users get it—assuming they're running your code. If this is a library others depend on, you're responsible for communicating the severity and ensuring they update.

#### New Features

A year in, users ask for schema validation against external references. Your framework doesn't support it yet. You can build it. Or you can document that it's not supported. Or you can suggest a workaround. The feature roadmap is yours to define. This is freedom. It's also burden. Every feature request requires you to decide: is this core to what we built, or is this scope creep? Can we afford to maintain this? Do we have the expertise?

---

### But what if we decided to use an existing solution instead?

You're building the same framework to validate and serialize/deserialize JSON requests and responses. The decision is made—you're going to use Pydantic.

#### Speed of Delivery

You spend an hour reading Pydantic's documentation. You install it. By afternoon, you have your first working endpoint using Pydantic's BaseModel. It's fast. Your validation logic is declarative—you define a class, Pydantic handles the rest. You didn't write any validation code; you wrote configuration. A week in, you're further along than Timeline A. Less code written by you means fewer bugs introduced by you. Your team picks it up quickly because Pydantic is well-documented and widely known in the Python ecosystem.

#### Control & Fit

Pydantic does most of what you need out of the box. It validates. It serializes. It deserializes. It's elegant. But it also has opinions. Your error messages come from Pydantic—they're good, but they're not _yours_. Your serialization follows Pydantic conventions—they're sensible, but they're not customized to your business.

Then you hit a requirement: you need custom validation logic that doesn't quite fit Pydantic's validator pattern. You work around it. Then another requirement: you need to serialize a field differently based on context (who's viewing it). You layer on logic around Pydantic. You're not fighting the library, but you're not in complete control either. You're working _with_ someone else's design, which usually works great—until it doesn't quite fit.

#### Documentation and Support

When your teammate asks "How do I add a custom validator?" you point them to Pydantic's documentation. It's thorough and well-maintained. If they have questions, they can search Stack Overflow—there are thousands of answers. If they're stuck, there's a Pydantic Discord community with active members. You're not the support bottleneck; the community is.

When a new team member joins, they can learn Pydantic independently. It's a popular library with extensive tutorials and courses. Your documentation burden is lighter because you're leveraging someone else's work. You only need to document _how your team uses Pydantic_, not the entire validation framework itself.

But there's a flip side: if Pydantic's documentation doesn't explain something well, or if it's outdated, you're stuck. You might need to read Pydantic's source code to understand a behavior. You might need to dig into GitHub issues to find answers. You're reliant on the quality of the documentation the maintainers provide. Most of the time, it's excellent. Sometimes, it's a gap.

#### Bug Fixes

A user reports that Pydantic isn't handling a specific edge case with your custom validators. You file an issue on GitHub. The Pydantic maintainers respond in a few days. Maybe the issue is already fixed in the development branch. Maybe it's a known limitation. Maybe it's a bug that needs investigation. You wait. Your fix is on someone else's schedule. If it's critical, you might need to monkey-patch Pydantic locally or work around the issue in your application code. You're no longer fully in control of problem resolution—you're dependent on the maintainers' priorities and capacity.

#### Security Updates

Pydantic announces a security vulnerability in its JSON parsing. You don't need to discover it yourself; the maintainers found it and released a patch. You update Pydantic. Your code doesn't change. The fix is handled for you. But you still have to stay aware of releases, evaluate whether you need to update, and test that the new version doesn't break your code. You're not responsible for finding the vulnerability, but you are responsible for applying the fix. You have less security work, but you can't ignore it entirely.

#### New Features

Pydantic releases a major version with powerful new capabilities—discriminated unions, JSON schema generation, async validation. You benefit automatically if you upgrade. You don't need to build these features; they're there for you. But you're also constrained by what Pydantic's maintainers prioritize. If they don't add a feature you need, you have to work around it, request it (and wait), or consider switching libraries. Your roadmap is partially defined by someone else's decisions.

---

### Reflection

In Timeline A, you traded _initial_ speed for long-term control and customization. Every advantage comes with an invisible responsibility: bug fixes, security monitoring, feature planning, maintenance burden—_and_ the obligation to document and support your own creation, whether for your team or for external users.

In Timeline B, you gained _immediate_ velocity and offloaded responsibility to the maintainers and the community. Every advantage comes with a tradeoff: you're working within someone else's constraints, dependent on their release schedule, and somewhat insulated from—but not exempt from—the underlying problems your code depends on. But you also gain the benefit of an existing knowledge base and community support, reducing your own documentation and support burden.
