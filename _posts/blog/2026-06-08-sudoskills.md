---
layout: post
title: Project 01 - SudoSkills
categories: [Blog]
tags: [build]
book: false
---

## The Project

[SudoSkills](https://sudoskills.geraldtui.com/) is a web application that allows you to learn Linux commands by typing them in a terminal. It's a fun way to learn and it's a great way to practice your linux command line skills.

## Concept

This started as a deep dive into websockets. The idea was simple: spin up a Linux terminal right in the browser and wire it to a real Linux machine on the backend. Each machine with different troubleshooting scenarios to practice. For example, a machine with a broken network interface, a machine with a broken filesystem, a machine with a broken service, etc. After 6 years away from a Linux Admin role, it was about time to stretch the old Linux muscle memory.

## System Design

"Simple". Yeah, about that 😅. While websockets itself is simple enough to grasp, the entire concept of curating sandbox environments and creating troubleshooting scenarios comes with its fair share of complexities. Hosted services, containers to orchestrate, lab scenarios to manage, container limitations and scalability, just to name a few. I managed to learn alot about websockets but it wasnt the right fit for this project. The project scope started creeping and before I knew it, I was violating Rule #1 of [10 Projects in Public](https://blog.geraldtui.com/posts/Ten-Projects/): Everything is an MVP (static sites or free tier tools).

So I flipped the whole thing client-side. There's no backend. SudoSkills is a Next.js app (TypeScript, Tailwind) that simulates Linux entirely in the browser. Instead of a real shell, there's a small engine: an in-memory virtual filesystem, a registry of command handlers (`ls`, `cd`, `grep`, `chmod`, pipes, the usual suspects), and a validator that checks whether your command produced the expected result for each lesson step. The terminal isn't something like [xterm.js](https://xtermjs.org/) either, just a custom React component with a hidden input that captures keystrokes and renders the output lines.

<iframe
  src="/assets/diagrams/sudoskills-architecture.html"
  title="SudoSkills system architecture diagram"
  loading="lazy"
  allow="fullscreen"
  allowfullscreen
  scrolling="no"
  style="display:block; width:100%; aspect-ratio: 16 / 11; border:0; border-radius:14px; background:#09090b;"
></iframe>

Lessons are plain TypeScript data files. Each step ships its own starting filesystem, so when you advance, the engine just reloads a fresh tree. Progress lives in `localStorage`. That's it. The payoff is that the whole thing builds to static files and deploys to Github Pages with zero costs.

I didn't ditch the websocket idea completely. It lives in a roadmap doc as a future "practice mode" with real machines. But for actually learning commands, the simulation does the job, and I get to ship something that costs nothing to keep online.

## Learnings

1. **Monolith vs Microservices:** Microservice architectures are not necessary at the beginning. Great for scaling but overkill for a proof of concept. Alex Xu makes the case for going monolithic early in [*System Design Interview – An Insider's Guide (Volume 1)*](https://bytes.usc.edu/~saty/courses/docs/data/): `"In a monolithic architecture, everything is bundled together: business logic, data access, and user interface. This is simple and effective for small-scale applications."` Monolith for now, microservices for later.

2. **Theory vs Practice:** I assumed learning Linux meant spinning up real machines. While thats certainly one way to learn, it's not the only way. [RegexLearn](https://regexlearn.com/) teaches regex in a simulated environment with no server behind it. A total beginner mostly needs the theory, what a command does and what output to expect, and a simulation handles that fine. Cheaper, safer, instant to load. That's the POC for SudoSkills. Real machines still matter, just not right now. Hands-on practice with containers or VMs sits on the roadmap and we'll cross that complexity bridge if we get there.

3. **Docker vs Virtual Machines:** Docker is tempting for sandboxed labs, they're cheap and fast to spin up. But a container isn't a machine. It's just a process [sharing the host's kernel](https://www.docker.com/resources/what-container/), while a VM runs a full OS with its own kernel. That sharing is the whole catch. Anything that touches the kernel, you can't really simulate. A few scenarios that fall apart: triggering a kernel panic to practice reading a crash dump, loading or debugging a kernel module, and tuning kernel parameters via `sysctl` that are namespaced or read-only inside a container. Networking gets awkward too. Real labs want things like simulating packet loss, a downed interface, or routing failures, and inside a container you're constrained by the host's network stack and whatever the daemon hands you. For that level of realism you need an actual VM with its own kernel.

## SudoSkills Future Roadmap

1. Practice mode with real machines.
2. Integrate Linux History into the lessons
3. Create Categories of Lessons: System Administration, Networking, Security, etc.

Thats 1 of [10 Projects in Public](https://blog.geraldtui.com/posts/Ten-Projects/), 9 to go!

GT <br>
Let's connect on [LinkedIn](https://www.linkedin.com/in/geraldtui/)
