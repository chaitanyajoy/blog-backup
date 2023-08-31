---
title: "Make Dirty Code Say 'I'm Clean'"
datePublished: Thu Aug 31 2023 14:52:04 GMT+0000 (Coordinated Universal Time)
cuid: cllzabi6q000f0akyaow0ejjh
slug: make-dirty-code-say-im-clean
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693493614307/71b8e490-9bc3-42dc-bc37-b20ec2c4f005.png
tags: design-patterns, clean-code, clean-architecture, solid-principles

---

Your code will be consumed only by two.

Machines and humans. period. *(I don't believe in Aliens)*

Machines look for the right syntax.

That's all. They can grind your code into binary within seconds.

But, what do humans look for?

When a fellow dev looks at your code, all he intends is to understand it as quickly as possible. If your code isn't self-explanatory at a high level, you're in conflict with fulfilling the purpose of a programming language.

Why? Let's see.

### Human first, Machine next

Programming languages are created to enable humans to communicate with machines. If not, we would write a long chain of 10111001s, asking the machine to do an `addition`.

The focus is human.

Let's say you resigned, and a new dev wants to extend your code by adding a feature to `subtract`. Certainly, he would try to understand your code first and proceed to his part. The chain continues and the `addition` you write today could be scaled to `algebra` by several developers in the next ten years.

Now, tell me, Isn't necessary to write a human-first code?

A machine can't fix a bug or add features by itself ([even the AI can't](https://deepak.blog/is-chat-gpt-here-to-take-our-jobs/)). Humans need to drive. When you write bad code, you're screwing others time.

In a [survey](https://solutions.insight.com/Resources/Featured-Items/Where-Leaders-Stand-in-2023) of 400 IT executives, 86% reported having been impacted by technical debt in the last year. IDG defines tech debt as “*the measure of the cost of reworking a solution caused by choosing an easy, yet limited, solution.”*

![Image by buildplease.com](https://buildplease.com/img/anothernewcoder.png align="center")

### Clean Code: The Motivation

One machine can run an entire codebase, but one human can't write it entirely. That's the reason we all operate as a team.

In our Church, a worship leader sings, and when he shows a fist, the keyboard & drum players will end the song. If he shows 1 finger, players will sustain for one more stanza. Here, the point is not fingers or fists, but the understanding they brought in the team to render a great melody.

Similarly, clean code principles and patterns are not the point, but the understanding they bring to the team is.

The motivation for clean code is to help teams communicate better and produce maintainable code.

### Clean Code: The Intention

In general, teammates carry two intentions.

**—** "Let's get the job done" and "Let's get the job done **right ✓"**

The second guy won't end his mission on a successful machine run but he ensures to support teammates by committing a clean code that saves a lot of 🕐 & 💰 for the business.

Again, getting the job done right is subjective.

Just because I think my code is clean, it will not become clean.

I produce solutions based on what I already know. When you perceive them, differences arise, because you respond out of what you know.

These differences of *"I'm right",* can escalate to heated discussions, and monopoly solutions, paralyzed approaches which result in a big ball of mud.

![No alt text provided for this image](https://media.licdn.com/dms/image/C4E12AQGrn52EQFtbyw/article-inline_image-shrink_1500_2232/0/1647758492658?e=1698883200&v=beta&t=z5uGV_m6LD4t02XmK9ZG3gStTm73UEwg_jK2EZEwGh0 align="left")

After all, such differences broke wars among the countries.

How can you and I be exempt?

But, think, how did the wars end?

Countries sit, talk, and sign a peace treaty, an agreement.

Similarly, we need a proven and common standard that was agreed upon and practised by a large number of software engineers.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">The SOLID principles, design patterns, etc. that serve the idea of clean code objective were time-tested, followed by the world's top companies and endorsed to be effective. Moreover, they were agreed upon by large communities of software development.</div>
</div>

Now, you understand why clean code? Let's see, How?

### First Step, Learn it Right

Half knowledge is more dangerous than no knowledge.

Before you apply, learn it right.

1. **Read The Docs:** Read through code standard docs of companies like [Google](https://google.github.io/styleguide/), Meta, etc. It costs nothing but a little humility to learn from others.
    
2. **Read The Code:** Give a thought on why OOP exist in the first place. Browse through top code repos in Git Hub and spend time skimming the code.
    
3. **Learn The Principles**
    
    * SOLID
        
        * **S**ingle Responsibility Principle
            
        * **O**pen-Close Principle
            
        * **L**iskov Substitution Principle
            
        * **I**nterface Segregation Principle
            
        * **D**ependency Inversion Principle
            
    * Design Patterns - [learn from here](https://refactoring.guru/design-patterns/what-is-pattern)
        
4. **Watch Workshops:** Check out the [sessions by Victor Rentea](https://www.youtube.com/watch?v=J4OIo4T7I_E), he influenced me a lot in upskilling my design & architecture skills.
    

Well, whatever you learn, should lead you to solving the problem in the simplest way possible.

Complexity is a beginner thing.

## KISS (Not What You Think)

KISS is a design ***principle*** abbreviated as "Keep It Simple, Stupid"

You know it can be done with just an if else statement. Why use lambda?

Maybe to show off others, or impress PR reviewers? Stick to simplicity.

The KISS principle says that we should not add unnecessary complexity to our software. Instead, we should keep our code simple and focused on the task at hand.

Often, your system design will determine the future fate of the project. No matter how good the later programmer can be, he can't fight your bad code once it accumulates the fat.

That's when your legacy will be trolled and become a [source of programming humour](https://www.reddit.com/r/ProgrammerHumor/comments/xa2rl3/legacy_code/)

Anyway, let's continue and talk some code.

## **Variables**

Prioritize readability for clarity.

If you name them right, they will speak the context.

You write `a = 3`, the name 'a' is saying just 'a'. If you write `speed = 3`, the variable is talking about a moving system and draws a question about the unit which can be assisted with a comment (`speed = 3 #in km`)

Clarity is a craft.

Choose descriptive names, format consistently, and structure logically, letting the developers grasp your code swiftly.

```python
# Less Readable
def calc_avg(nums):
    s = sum(nums)
    c = len(nums)
    a = s / c
    return a

# More Readable
def calculate_average(numbers):
    total = sum(numbers)
    count = len(numbers)
    average = total / count
    return average
```

## **Functions**

Prioritize conciseness over complexity.

Compact functions with a single responsibility enhance code maintainability. Dividing complex tasks into smaller chunks yields focused functions that simplify debugging and modifications.

```python
// Long Function
def processUserData(user) {
    // ... process data ...
    // ... update database ...
    // ... send notifications ...
}

// Concise Functions
def processUserData(user) {
    processUser(user);
    updateDatabase(user);
    notifyUser(user);
}
```

*Fun fact: If you made reading till this point, I know you fall into the "Let's get the job done right" category.*

## **Comments**

Meant to give insights.

Comments are helpful when they provide insights that code can't convey. Use them sparingly to clarify intent, not just repeat what the code does.

```python
# Comment Overload
def calculate_total_price(products):
    # Loop through products
    # Calculate price for each product
    # Add to total_price
    total_price = 0
    for product in products:
        total_price += product.price
    return total_price

# Precise Comment
def calculate_total_price(products):
    total_price = 0
    for product in products:
        total_price += product.price
    return total_price
```

## **DRY (Don't Repeat Yourself)**

Embrace this tightly.

I am not a fan of Microsoft's products but I like a quote by its founder.

> "Measuring programming progress by lines of code is like measuring aircraft building progress by weight"
> 
> Bill Gates

Duplication leads to confusion and maintenance nightmares.

Extract repetitive logic into reusable functions or constants.

```python
# Duplicate Logic
def is_valid_email(email):
    if "@" in email:
        return True
    return False

# Reusable Function
def is_valid_email(email):
    return "@" in email
```

## **Version Control**

Use version control to track changes systematically. Each commit is a checkpoint that allows you to navigate through code history and collaborate effectively.

```python
# Commit Message
git commit -m "Refactor authentication logic for improved readability"
```

## **Test — Arrange, Act, Assert**

End of the day I'm an SDET.

I have to speak about tests. While I can't comprehend everything now (Which I plan to do in future), all I can say is to arrange, act, and assert.

Avoid big setups and data manipulations at the test file level.

Recently, I got to write some example tests, check them out below:

* Java with Selenium - [check the tests](https://github.com/chaitanyajoy/FullCreative/blob/master/src/main/java/io/full/htmlcanvasstudio/CanvasTests.java)
    
* JS with Cypress - [check the tests](https://github.com/chaitanyajoy/Cypress-Shopist.io/blob/master/cypress/integration/e2e/cart.spec.js)
    

```python
# Well-Structured Test
def test_user_authentication_success():
    # Arrange
    user = create_user()
    
    # Act
    authenticated = authenticate_user(user)
    
    # Assert
    assert authenticated

# Avoid Complex Setup
def test_complicated_scenario():
    # ... complex setup ...
    # ... hard-to-follow assertions ...
```

## **So, In Conclusion**

In case, you are inspired too much to learn and have plans to revamp your company's prod code tomorrow, I have to slow you down.

Remember, having learned about patterns, your brain tricks you into trying to apply them everywhere, even in situations where simpler code would do just fine.

If all you have is a hammer, everything looks like a nail.

At the same time, clean coding isn't just a preference, it is a pragmatic approach to craft code that shines in the hands of fellow developers.

By the way, I write about SDET practices, testing strategies, and code optimization that refine our skills in the world of QA.

Let's be in touch, hit subscribe.

See ya.