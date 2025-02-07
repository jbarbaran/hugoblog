+++
date = '2016-08-21T17:34:02+01:00'
draft = false
title = 'Clean code'
lang=  'en'
+++

![Portada del libro Clean Code](/images/cleancode.webp)

In all productive aspects of society, product quality is essential. When it comes to software, it's exactly the same—except that software is a special kind of product. For this reason, in most cases, the client cannot or does not know how to evaluate the quality of the delivered product. They only check whether it meets the specified requirements in terms of appearance and functionality, without considering how it was built or its level of quality. However, the quality of developed software is crucial to prevent future errors and ensure that adding or modifying functionality is not a nightmare or excessively costly, among other reasons.

In software development companies, it is common practice to impose deadlines for projects that are generally unachievable for development teams—whether due to client commitments, costs, or other reasons. As a result, teams are pressured to develop software as quickly as possible to minimize delivery delays (because, let’s face it, there will be delays). To meet these deadlines, developers often sacrifice essential development practices that ensure a minimum level of quality, such as Test-Driven Development (TDD), the use of continuous integration tools, or refactoring the code as it is written. These practices help eliminate code duplication, improve readability, and follow principles such as the Single Responsibility Principle (SRP), among many others that make up what is now known as Clean Code.

This is bad news—very bad news. The result is that a software product will be delivered functionally in a short time and with minimal staff, but at what cost? The developed software will be incredibly difficult to maintain, there will be no unit tests to verify system functionality (meaning that adding new features may break something else without anyone realizing), and code readability will likely be questionable at best.

On [Javier Garzas's blog](http://www.javiergarzas.com/) (highly recommended), I remember reading a post stating—very accurately—that the role of software engineers is to passionately advocate to their companies that software quality is not just recommended but essential. Yes, ensuring quality means that development may initially be slower, but the long-term benefits are enormous. The post also included an analogy—a bit extreme, but it clearly illustrates the point:

A patient about to undergo an appendectomy, already in the operating room, sees the surgeon washing their hands and instruments and says:
*- "Come on, doctor, don’t wash your hands, it doesn’t matter, just operate on me now! I’ll pay you, and I say there’s no time to waste." -*

Of course, the surgeon, as part of their duty, refuses the request and insists on sterilizing everything before proceeding. Yes, this takes extra time, but skipping it would likely lead to severe post-operative complications.

And that is precisely the development team's mission when dealing with their company: to emphasize the importance of Clean Code or to accept the consequences of rushed, low-quality development.

For those who are still unsure—what exactly is Clean Code? A quick search on the internet will bring up Wikipedia, blogs, and plenty of articles on the topic. But I found a book titled simply Clean Code. It's available on Amazon at this [link](https://www.amazon.es/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882/ref=tmm_pap_swatch_0?_encoding=UTF8&qid=&sr=). I checked the reviews (all excellent) and bought it without hesitation. As the title suggests, this book makes a strong statement of intent. And I have to say—it does not disappoint.

In my opinion, the book is incredibly thorough, covering all aspects of good programming with clear examples of what should and shouldn’t be done. Some may argue that these concepts are taught in computer science degrees, but nowadays, more and more programmers come from non-CS backgrounds. And even if you've learned these principles before, it’s always good to have them fresh in your mind. That’s why I firmly believe this book should be required reading for all software professionals—whether they have a formal CS background or not.

Beyond Clean Code, the book also covers topics such as concurrency and design patterns. Some of its advice may seem trivial to experienced developers, but these reminders are incredibly important—especially in a corporate environment where new features and changes are constantly demanded yesterday. The goal? To write Clean Code and take pride in what we build.

Is this the only book on the subject? Not at all. There are countless great books and blog posts on Clean Code, Test-Driven Development (TDD), and other methodologies. However, I personally found this book particularly clear, direct, and, above all, useful.