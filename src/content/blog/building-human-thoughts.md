---
title: "Building the Blog That Publishes the Methodology"
date: 2026-03-26
---

## The Irony Was Not Lost on Me

The first post on this blog is a whitepaper about staying in the conversation with AI tools. About not delegating the understanding and being in the room when the decisions get made.

The blog it lives on was built using that methodology. **E²** in practice, from the first question to the last deploy.

I walked into the session not knowing Astro, Umami, hCaptcha, Giscus, or how GitHub Pages DNS actually works. I walked out knowing all of them. Not just that they exist, not just enough to cargo-cult a config file. I know why they were chosen, how they help solve the core problem statement, and what I would tell someone else before they start. That knowledge lives in me now, not in a prompt I can barely remember writing. It lives there because I was **engaged**.

This post is about how that happened. Not the methodology in the abstract. The actual session. The decisions, the problems, and the moments where conversation produced something that delegation never would have.

If you have not read the whitepaper yet, go read it, I'll wait. This post will make more sense with that context. But if you just want the practical stuff, keep going. Every section here is a real decision point with a real takeaway, but reader beware: it requires some acceptance that just because you can get a working output from a tool with little effort, doesnt mean you get something that represents a creative vision.

---

## Astro: Finding a Tool I Would Never Have Googled

Before the session started, I had put together a loose task document. Not a spec. More like a napkin sketch. Dark mode blog. Fraunces for the typeface, JetBrains Mono for code. An about page. Contact form. The broad strokes of what I wanted, with none of the details filled in. The kind of plan you write when you know the destination but not the route.

The starting point for the conversation was simple: I have markdown files and I want them on the internet. Rendered, hosted via GitHub Pages. What are my options?

If I had gone to Google with that question, I know exactly what would have happened. "Markdown blog GitHub Pages" returns Jekyll. Every result. Jekyll is what GitHub Pages uses natively, so the entire first page pushes you toward it. You would never see Astro. Search "markdown renderer static site" and you get MkDocs, a handful of small GitHub projects, and documentation tools. Still no Astro.

Astro lives in a different search space. "Modern frontend frameworks." "Astro vs Next.js." Searches you would only run if you already knew it existed. The tool that turned out to be the best fit for my project is essentially invisible to someone googling the problem it solves.

But I was not googling. I was in a conversation. I described what I needed, and the list that came back included Jekyll, Hugo, Gatsby, Next.js, and Astro. I had never heard of Astro. My entire mental model of frontend frameworks was React, Svelte, that general space. So I asked a question that might sound embarrassingly basic:

> "Is this a framework or is it a UI component we IMPORT into a frontend library like React or Svelte, or is it something else?"

Turns out Astro is not a component library. It is a full framework. It builds to plain HTML with no client-side runtime by default. Components render at build time and ship as static markup unless you explicitly opt into client-side interactivity.

That is what sold me. I am not building a web app or a SaaS. I am publishing long-form writing. I need fast page loads, good SEO, and markdown rendered to HTML. Astro does that natively. The other options could too, but Astro was built for exactly this use case. One question about what it actually is, and the choice was clear.

**The takeaway:** Search engines return what you know how to search for. **E²** conversations surface what you do not know to search for. If I had googled my way into this project, I would be running Jekyll right now. It would work fine. But I would have missed the better tool because I did not know it existed, and Google was never going to tell me.

---

## Paper on the Desk: When Design Emerges from Conversation

My task doc said dark mode. That was the whole design direction. If I had handed it off and said "build this," I would have gotten dark background, light text, standard layout. Functional and forgettable.

Instead, I started talking through what the reading experience should feel like. And something clicked:

> "If our bg is dark, charcoal, burnt, I'm thinking the text canvas is sort of like a muted white or muted cream, creating a sort of 'paper' effect. Inspiration was typewriter style lettering, with dark-mode code blocks and accent blocks and quote blocks."

That idea did not exist before the conversation. A cream writing surface floating on a dark desk, with dark accent blocks for code and quotes creating contrast within the content. It handled readability, because dark text on light paper is easier on the eyes for long reading. It handled content boundaries, because the paper edge is the boundary. And it gave the site a point of view instead of a default theme.

That concept cascaded through everything. The header became ghosted, barely visible until you hover, because the paper is the star and everything else should get out of the way. I called it a "zen-mode effect."

It also solved a problem I had been chewing on about fonts. My task doc said Fraunces for headings and body, but my immediate reaction was:

> "I hate when a blog site has the same font EVERYWHERE. Like, where does the post stop and the BS begin?"

I was ready to go find a second font for the site. But then I realized the paper concept already handled it. Fraunces in dark ink on cream paper feels completely different from Fraunces in muted light text on a charcoal background. Same typeface, different universe. The distinction is built into the layout, not the font stack.

My task doc also had a separate `/about` page. During the conversation I asked whether we should inject an author card into every post instead. Write once, use everywhere. That eliminated an entire page from the site, simplified the navigation, and put the "who wrote this" context exactly where readers would want it: right after the content, before the comments.

**The takeaway:** A loose plan plus a real conversation will outperform a detailed spec handed off in silence. The paper concept, the zen-mode header, the font realization, the author card — none of those were in my task doc. They could not have been, because they did not exist until the conversation produced them. You cannot anticipate what you have not thought of yet. Conversation is how you think of it.

---

## Analytics and Comments: Fast Decisions, Right Decisions

Not every decision needs to be a long deliberation. Sometimes the value is in seeing the right options presented honestly so you can move fast with confidence.

For analytics, the choice was between Umami and Plausible. Both are privacy-respecting, both are open source, both give you what you need without selling your visitors' data to ad networks. Umami is self-hostable and free. Plausible has a managed service with a clean UI. For a personal blog where I wanted full control and zero recurring cost, Umami was the obvious pick. The conversation confirmed what I was already leaning toward and added context about deployment options. Decision made, moving on.

Comments were even faster. The options were Giscus, Disqus, lightweight emoji reactions, or skipping comments entirely for v1. When Giscus was described, my response was:

> "Oh, Giscus is EXACTLY what we want."

Giscus maps comments to GitHub Discussions. Readers authenticate with GitHub. No separate account system, no moderation nightmare, no Disqus tracking scripts. For a technical blog whose audience is developers, this is a perfect fit. I knew it the second I saw it described.

**The takeaway:** **E²** is not about deliberating on everything. It is about having enough context that when the right answer is obvious, you recognize it immediately and do not waste time second-guessing. Fast decisions are still informed decisions if you understand the tradeoffs.

---

## The Cloudflare Disaster and the hCaptcha Pivot

This is the section where things went wrong, and where being in the conversation saved hours.

The contact form started simple. A POST to a FastAPI backend on Railway. It worked. Then I asked a question that was not in any task document:

> "What sec do we have for the BE?"

I was feeding my 4 month old at the time, so I had to short-hand it a bit, typing with one hand is hard.

Anyways, that single question surfaced three gaps: no rate limiting, no input length validation, no bot protection. None of these were pre-planned to be dealt with. They came up because I was thinking about the thing I had just built, not just whether it technically functioned. It didn't get missed because I didn't depend on the tool to assume to fix it, or find it. That is **E²** — the human asking the questions the tool will not ask itself.

Rate limiting was straightforward. Slowapi, a few lines of config. Input validation was already half-handled by Pydantic. The captcha is where it got interesting.

The initial choice was Cloudflare Turnstile. Free, invisible, privacy-friendly. CloudFlare wanted my domain to be part of my Cloudflare account. That is a problem. I keep my domains on Namecheap. I manage my DNS through Namecheap. I am not a DNS wizard, and DNS is still relatively new territory for me. A captcha solution that requires me to move or mirror my domain management into a second platform is not a simple add. It is an ecosystem commitment.

And Cloudflare IS an ecosystem. It wants you to live in it. CDN, DNS, DDoS protection, Workers, Pages, Turnstile. Each piece works best when all the other pieces are also Cloudflare. That is a fine choice for someone who wants that. I do not. I needed a captcha widget. I did not need an infrastructure platform.

The agent did its best to solve the problem, but eventually it was just making assumptions and taking shots in the dark. So I did what **E²** says you should do in this situation: I hit the escape key.

> "I am willing to bet money this is because the domain is not registered with or verified on Cloudflare. Personally, I don't need an ecosystem, I need a captcha widget. What other options can we consider?"

The pivot to hCaptcha took minutes. New widget on the frontend, new verification endpoint on the backend, new API keys, redeploy. Done. hCaptcha does not care who manages your DNS. You sign up, you get keys, you drop it in. That is what a captcha integration should feel like. Keeping this in my back pocket for future projects!

The reason I could make that call fast is because I understood the full picture. I had been in the conversation for the architecture, the DNS setup, the deployment. I knew what Turnstile was asking for and why it was asking for it. And I knew it was asking for more than the problem required. A reactive approach would have looked different. Agent or multiple agents run until the task is "acomplished". The widget is broken. Debug it. Try different Cloudflare settings. Google the error code. Try again. An hour disappears into a platform onboarding flow, before you know it your wasit deep into a ecosystem you never really wanted, or needed. The tool is not going to tell you when to give up. That is a human judgment call.

**The takeaway:** Knowing when to abandon a path is a **skill**, and it requires understanding the path well enough to recognize when it is a dead end versus when it is just hard. Being prsent so you can speak up asks that you stay **engaged**.

---

## Giscus Theming: Steering, Not Reviewing

If I had delegated the blog build and gotten a result back, the comments section would have been the first thing I asked to change. It would have looked like default Giscus. Fine, functional, completely disconnected from the rest of the site. I would have filed a note, waited for a revision, looked at it again, filed another note. The back-and-forth of reviewing someone else's output.

That is not what happened. I was in the conversation when Giscus went in, so I was steering the visual direction from the start.

Giscus renders inside a cross-origin iframe. The CSS that styles it gets fetched by that iframe from a URL you provide, not injected by your page. That means your browser dev tools cannot easily inspect it, and it means local development is weird because the iframe pulls from a URL, not your local files. Fun times! Reminds me of a project I worked on that once received font and style rules from an inbound SAML token.

I wanted the comments to match the blog's paper aesthetic. On localhost, they looked wrong. Different from production. The agent explained that the iframe fetches its CSS from whatever URL you provide in the config, and since we had pointed it at the production URL, it was pulling the live stylesheet — not the one I was editing locally.

That clicked immediately. The iframe is not reading my files. It is making an HTTP request to a URL, and that URL points at the last thing I deployed.

> "You're saying that because we told it to pull from our website, it's not finding the changes locally? In dev can't we pull from a local file instead?"

We tried. The giscus iframe is served over HTTPS from giscus.app. Pointing it at `http://localhost` is a mixed content request — the browser blocks it. The iframe literally cannot reach your local files. Knowing that constraint meant I did not waste time trying to force it. This themeing was a once and done task, so no need to save the world here. We ignored theme for local dev and focused on the custom CSS on production. Problem understood, solution chosen, moving on.

That understanding is why the refinement went fast. I was looking at the live site and giving real-time direction:

> "That text should match body text bolded."
> "Borders are still too thin, let's go for 5px."
> "This section should look JUST LIKE / MATCH our body text and blog post body."

Those are not revision notes on a deliverable. Those are steering inputs during the build. The agent had the CSS knowledge to make a cross-origin iframe obey. I had the vision for what the site should look like. We got there in minutes because there was no handoff delay, no "let me take a look and get back to you." I said what I wanted, it happened, I looked at it, I adjusted. Tight loop. Done.

If I had delegated this, the comment section would have taken way to much back-and-forth revisions to get right. Expensive token bloat for no reason, just so an agent can do the steering 10x times worse than I did. If the agents even figured out to just style against and check production. That is what **E²** gets you. Not just a better result. A **measurably faster** one.

**The takeaway:** Do not delegate visual decisions and hope they come back right. Be in the conversation when they happen. You will iterate faster, communicate more clearly, and end up with something that actually matches what you had in your head. The tighter the feedback loop, the fewer revisions you need.

---

## What I Walked In With vs. What I Walked Out With

I walked into this session with a markdown file and a vague idea that I wanted it on the internet. I had never touched Astro. I did not know what Giscus was. I had never configured Umami. I thought Cloudflare Turnstile would just work. I had no design beyond "dark mode, I guess."

I walked out with a live blog that has a coherent design philosophy, environment-aware analytics, a captcha that actually works, comment theming that matches the site, and a contact form with real security considerations addressed. More importantly, I walked out understanding all of it. Not at an expert level. At a "I know what this does, why it is here, and what I would change next" level. The level that lets you come back in three months and keep building instead of starting over.

Every piece of that understanding came from conversation. From asking what Astro actually is instead of accepting it from a spec. From thinking out loud about what a reading experience should feel like and landing on the paper concept. From asking "what sec do we have" after the feature worked but before it was done. From recognizing that Cloudflare was a dead end in real time instead of an hour later.

**E²** argues that the discussion is the diagnostic. That understanding accumulates through **engagement**. That the human's presence in the process is what makes the output **theirs**.

This blog is what that looks like in practice. Not because it is perfect. Because the person who built it knows where every seam is, and knows what to do about each one.
