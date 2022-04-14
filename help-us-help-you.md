# How to ask a question

One of the most important takeaways from this course is how to frame your requests to reduce back-and-forth with developers. In other words: how to ask productive questions.

Whether you're asking your classmates and instructors for help with your homework, or requesting a bugfix from your team, it is always appreciated when you...

![](/assets/helpmehelpyou.gif)

... help _us_ help _you_.

Please try to keep the following[^stack_overflow] in mind when you're asking questions:

[^stack_overflow]: Some portions inspired by [StackOverflow's "How do I ask a good question?"](https://stackoverflow.com/help/how-to-ask){:target="_blank"}

## Asked and answered

Many times, your question has already been asked and answered. Scan the existing questions before you write up your question; even if your exact question isn't already there, you'll probably learn something.

To make easier for everyone to find relevant answers, when you are _asking_ questions:

 - Consider making the question **public** rather than private, unless it truly is something unique to only you (e.g. a request to swap sections is an appropriate thing to make private).

    Any question related to HTML, CSS, Ruby, Rails, Heroku, etc, will probably be helpful to more people than just you — you should make it public.
    
    You can make your question public and still choose to be anonymous to classmates, or anonymous to everyone. Feel free to do that.
 - To make it easier for others to find useful information when scanning the list of questions, it really helps to **write a good title**. See next section.

## Write a title that summarizes the specific problem

- Pretend you've bumped into an instructor in the hallway and have to **summarize your entire question in one sentence**: what details can you include that will help someone identify and solve your problem?
- If you're having trouble summarizing the problem, **write the title last** - sometimes writing the rest of the question first can make it easier to describe the problem.

Examples:

 - **Bad:** Math Confusion
 - **Good:** Why does `3/5` give `0`?
 - **Bad:** HW5 doubt
 - **Good:** How can I redirect users to different pages based on session data?
 - **Bad:** if else problems
 - **Good:** Why does `x` evaluate to `true` when `x = 0`?

## Context is king

In the body of your question, give us _context_, not just code (and _definitely_ not just a link to a workspace snapshot).

A few potential items to consider including:

1. What project are you working on?
1. What action are you taking? e.g. "When trying to visit the URL /photos/4..." or "When I submit the form on my photos index page..."
1. What behavior are you expecting?
1. What behavior is happening instead?
2. If there is one, the detailed error message. Remember that [your skill as a developer](https://chapters.firstdraft.com/chapters/754#seriously-please-read-the-error-message){:target="blank"} is measured by the number of error messages that you've experienced and learned to resolve.
  - What is the headline error message? e.g. "undefined method 'capitalize' for 3:Integer". Note that I didn't say just "undefined method" or "undefined method 'capitalize'". Include the _whole_ error message — the last part is usually the most helpful.
  - What file did the error occur in?
  - On what line number?
  - Snippet of the code that is highlighted in the error message.
 1. If it's a Rails project, the server log for the request: what URL were you trying to access, which controller action processed it, what were the `params`? It is helpful to [Clear your terminal](https://chapters.firstdraft.com/chapters/834#clear-terminal){:target="_blank"} before reproducing the issue so that you have clean server log output to copy-paste.
 2. Screenshots are sometimes helpful to show things like CSS behavior, but please don't screenshot your code or error message.
    - Copy-paste your code into the question. Use the code block formatting tool, or [a fenced code block in Markdown](https://www.markdownguide.org/extended-syntax/#fenced-code-blocks){:target="_blank"}, so that your code displays correctly.
    - Type out the error message yourself — it'll force you to read it, which too often doesn't happen.
  1. Unusual circumstances that make your question different from similar questions that have already been posted.


## Bug bounty

## Snapshots

## Markdown

## Rubber ducking


3. It helps you think through the problem thoroughly, which often is enough to figure it out on your own. Almost all programming bugs are a mismatch between what you _think_ you're asking the computer do and what you're _actually_ asking the computer to do.
4. It gives us enough data upfront to make an educated guess about the problem and avoids a potentially time-consuming back-and-forth to gather all the relevant info.
