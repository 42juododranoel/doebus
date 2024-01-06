# Vsevolod Skripnik’s Developer Wisdom

This is a list of my opinionated rules and pieces of wisdom that I believe to be beneficial and important. I have experienced each of them many times, and I am 100% sure that they bring lots of value if utilized correctly. This list gets extended as I get new experiences. Be aware that this document is mostly written for small to medium-sized product companies — I rarely encounter outsourcing companies that share the values I will describe below. Also, remember that most of that is written with Python-powered web development in mind.


| Table of Contents |
| ----------------- |
| [Developer Experience & Abstract Wisdom](#developer-experience--abstract-wisdom) |
| [Architecture](#architecture) |
| [Python](#python) |
| [Pytest](#pytest) |
| [Django](#django) |


## Developer Experience & Abstract Wisdom


### Per-Team Repositories

It is crucial for a team of developers to feel some degree of ownership and responsibility for their repository. The backend team should have their backend repositories only for themselves — the same goes for the frontend, the infrastructure, and other teams. It is discouraged to mix teams together in one repository. I call this “a dormitory effect”: a repository that belongs to everybody belongs to nobody, and nobody is responsible for housekeeping in it. People generally like to care about quality and keep things clean and tidy, but only if they feel connected to the place. Developers should feel at home in their repositories.


### Per-Developer Repositories

If there are many repositories in your project, it is then generally a good idea to have a home repository for each developer. Each developer should be allowed to make things their way (to some limited degree) in their home repository. This may look controversial initially, but I’ve seen how this approach helped reduce the number of arguments to near zero and introduce a kind of “antifragility” to the code. In my experience, this approach allows developers to boost knowledge sharing and realize that valuable ideas can be found in any style or structure — even if they initially look weird. Keep in mind that this primarily works with middle+ experience levels. It will do more harm to teams with less experienced developers.


### Test Your Code

There are tons of pros to writing tests:

- It is easier to hire developers who value developer experience.
- Less recurring bugs.
- More accurate estimation of time requirements for tasks.
- Saving time and money on manual testing.
- Higher developer morale thanks to lower code complexity (if testing is done correctly).
- Being able to perform large-scale architecture reworks that would be unfeasible without tests.
- New developers learn projects faster.
- More trust between developers.
- Less stress for developers when software malfunctions.

I don’t remember who said it, but code without tests is legacy by design. There is a popular misconception that writing and maintaining tests takes longer than working without them, but in my experience, this only applies when your developers are not experienced enough with tests and testing culture is being actively overlooked. This situation changes drastically when your developers reach a certain point of proficiency in testing (i.e., knowing how to effectively perform complex mocking deep inside your project’s core or quickly write tens of tests that rarely exceed the size of 5-7 lines of code each). After getting skilled enough, the tests would stop requiring more time and instead will start saving your developers time.


### Use Linters

Linters are automated tools that highlight potential problems in your code following specific rules. The more rules your linters support — the better. Linters should be run on each build before tests, and the testing should not proceed without a hundred percent linting compliance. If your project is legacy and 100% compliance is unfeasible, try using (or writing) a special plugin for your linters that would count the current linting errors in the codebase and, at the very least, require each commit not to introduce any new linting errors, ideally it should also require each commit to reduce the amount of current errors at least by one. All linting on the project should be unified across all machines and ideally run “live” on each IDE. It is also a good idea to enforce a unified automated code formatting using a tool like Black in Python or Prettier in Node. Aside from linting, you should also strive to use type-checking. It may be tedious, but it is worth the effort and helps to improve your architecture skills when you practice thinking in types for a long enough time).


### Bulletproof Deployments

If the necessity calls for it, you may have bad architecture, tests, and code style, but you can never afford bad deployments — there is no other thing that kills developer morale as fast as code deployment problems. No other developer experience practice really matters if your code is not easily deployed. Your developers should always:

- know exactly which version is running on which server, 
- have a simple interface to deploy their code, 
- have easy access to deploy logs in case something fails.

Strive to make your deployments predictable, simple, bulletproof, and ideally even downgradable, but at the very least, they must be fully automated. 


### Code Review Practice

Each PR should be reviewed by one teammate of the same or higher seniority level. This rule is not required for seniors and tech leads but is mandatory for middle, junior, and intern developers. There are multiple benefits of code review: 

1. A second opinion may introduce a better solution to the problem than the original one. 
2. Developers learn from each other and become more productive in the long run.
3. It helps to reduce bugs and errors since all humans sometimes make mistakes by accident.

It should also be mandatory to strictly review junior- and intern-level developers as they often lack the experience to solve the problem in an acceptably efficient manner and without breaking something else around. 


### Product-Knowers

Whatever we do — if we get paid for it — a person (or a person’s representative) pays us to do that specific thing for them. This person may be called a product owner in a product company or a client’s manager in an outsourcing company, but I call these people the product-knowers. They are distinguished by a clear understanding of what kind of product we are building, why each particular task is important, and what the expected result should be. Ideally, each developer should start each task by making a bullet list of things to add, change, or remove, with all of that described in simple human language, and that list should be reviewed and accepted by the product-knower before the actual programming starts. During the programming part, the developer should have easy access to product-knowers, and ideally, each task should have a dedicated product-knower assigned to it (if there are multiple people in that role on the project). No task should ever be considered finished without getting the approval of its product-knower. 

Each developer should strive to understand as much as possible about the product, making their communication with product-knowers as brief as possible due to equal knowledge on both sides. This is not completely possible — and not desirable either — but it still should be a goal of each developer. This applies to both backend and frontend developers, but the frontend developers should also have the exact same type of communication with designers to make sure that the frontend’s UI and UX are solid and high-quality.


### QA Done Right

The ideal role for a QA on the project is to deeply understand the user behaviors and the product itself (being the second only to the product-knower), and at the same time, being able to help developers with simple tech things like JS requests or backend APIs (if we are building web apps) or DBs (if we are doing ETL, etc.) and so on. At the very least, your QA engineers should never be held responsible for developers’ mistakes, as it makes developers lazier and more infantile. Whatever the workflow is, it is ultimately the developer’s responsibility to make a quality product. 


### No Coverage Metrics

In my experience, coverage metrics are make-believe, and having 100% code coverage doesn’t equate to having 100% quality code or 100% product use cases working properly. Some code can’t or doesn’t even need to be covered at all. You may have it for convenience, but at all costs, avoid making coverage level a requirement. Otherwise, your developers will be constantly tempted to think less about the quality of their work and more about how to trick the coverage system into thinking that the work has high enough quality.


### Less Environments

It is controversial, but you don’t necessarily need local, dev, stage, and prod environments. Having only local and prod is enough and ideal — that‘s what my experience tells me. The more environments you have, the more complexity there is, and your work become less efficient. Having a dev server may aid in some cases (if your frontenders for some reason cannot use production servers as their backend, or if you need a place to run demonstrations, or if your production server has an incredibly complicated infrastructure, etc.) — but your dev server should be easy to maintain and recreate if something gets broken, and at the very least it should never be required for backend development.

You should always strive to make the difference between the local, dev, and prod environments as slim as possible, and every developer (ideally QA as well) should be able to run a dockerized copy of your project on their local machines with both backend and frontend. There is really no excuse now as literally anything can be automated and run with Docker. If you think that your infrastructure is too complex to be run like that — I still suggest trying extensive mocking and isolating before falling back to multiple environments.


### Positive F-wording Around

Your developers should be allowed to do some f-wording around (provided that it’s not too harmful). It is permissible to break your own rules sometimes, such as violating code style conventions in some places. The reason for that is that if you always do things this way or that way, you may become fragile and miss some important discoveries. Sometimes, the only way to find a solution for a problem is to f-word around it for long enough with utmost disrespect for any rules.


### On Stone Soups

The following paragraph is meant only for people who feel a genuine passion for either software development or managing software development projects and people. It is not meant for managers with controlling and trust issues and not meant for developers with lazy-ass syndrome. 

A good anecdote states that the junior doesn’t know how to do a certain thing, the middle knows how to do that thing, and the senior knows how not to do that thing. That’s legit wisdom, but there is more to it. In my book, the definition of a senior developer is being able to provide an abstraction over the development processes for non-technical people. When I am hired as a senior developer, my managers don’t need to know most of the things happening on the technical side: they will not hear from me such words as “isolation levels” and “cyclomatic complexity” — they only need the results — but the price is that the rule is also applied to such things as using linters or not, refactoring my code or not, writing tests or not, rewriting parts of the project or not, and trying new libraries or not. It may sound controversial (especially if you are reading this as a manager), but I believe that proficiency at cooking stone soups is a required skill for a senior developer — and a lead developer as well. I’ve been a manager and a lead developer in the past, and I have seen all sides here and saw no evil. Obviously, you are not hired to feel good and have fun doing tech stuff all the time — you are hired for a very specific purpose, and you obviously must make that purpose your top priority — but at the same time, I believe that as long as you serve that purpose, you are not morally obliged to report to your managers on every step and decision you make.


### Presentation & Reputation

“You are a villain, alright, just not a super one,” Megamind said, to which Titan replied, “Yeah, what’s the difference?” The camera zoomed in to the Megamind walking out of the giant flying head lit with lasers and blasting rock music. “Presentation!” he shouted. I believe this piece of wisdom generally holds for most jobs, but I — as a software developer — above all else, apply it to my kin. I believe that a software developer is a part-time politician (or a supervillain, if you will) and must use every chance they get to continuously build an image and present themselves as a competent professional who is there to kick ass and write code (and is all out of code). Imagine you are working on a coding task and struggling with some technical problems. Do you tell your manager about it during a daily stand-up meeting when they ask about your progress? No, you don’t. You only tell that when the problem is successfully solved. To be clear, I am not telling you to ignore problems — I am telling you to look like a person who solves problems more often than struggles with them. You will be struggling from time to time, as we all do, but your managers don’t necessarily need to see you struggling — what they need to see is that you successfully solve problems. Try not to miss chances to present things that people will most likely enjoy seeing, and try milking these chances as much as possible. People’s impression of you is built by your actions — and my advice is to make that impression impressive — it will serve you well. 

Another related piece of advanced wisdom here is the importance of managing the development team’s reputation in the eyes of all others — but most importantly, in the eyes of the project manager. I repeat: this one is extremely advanced and mostly rests on the shoulders of tech leads and senior developers. Have you heard developers whining about managers not understanding a damn thing about software engineering yet peaking their noses where they shouldn’t and making stupid decisions? That happens — I’ve seen it — but the managers do not do that out of pure evilness. Most of them would actually be happy never to have to do that, but the circumstances force them to, and the biggest circumstances are subpar, often lazy, childlike developers. There is a general mistrust between the managers and the developers, and I believe the developers should be actively working on reducing that mistrust as long as they want to be proficient and ass-kicking. How do you do that? In my experience, the following:

1. Keep your promises. Don’t promise something you can’t do.
2. If you can’t keep your promise — tell that in advance and propose a solution. It may not work, but you should always try. 
3. Always strive to improve your understanding of the project, domain, clients, and business. The more synchronized you are with business people — the better. Try to control your (most likely present) passion for coding per se and instead focus on (or at least try focusing on) what the company needs.
4. Constantly learn about the project’s deadlines, goals, and progress. Your manager should not be reminding you about such things, and instead, should sometimes be hearing from you how you went through the backlog and found something was missing there or double-checked some important features before tomorrow’s presentation, etc.
5. Never use technical lingo when talking to non-technical people. Try to make things as clear as possible to them.
6. Do not think that your (most likely) higher-than-average intelligence makes you a better-than-average person. Do not assume that people around you are stupid or evil — they are most likely not — but at least do not actively express your feelings about them and keep it neutral if you absolutely can’t do better.
7. Try to bring either good news or bad news followed by “but I have a plan”. Again, I repeat that I do not suggest hiding problems — I suggest a certain framing of how you speak about them. Remember also that good news is good not only because of what it is but also because of when it is. The same thing may make different impressions depending on how good the timing was.


### On Leadership

Should you exert the aura of confidence? Should you predict things before they happen? Should you be the one who is always with the plan? Should you be the most resilient, with the fortitude to carry on whatever happens? Should your mere presence inspire people? Should you be the most knowledgeable one or the one with the most years of experience? The one who always helps others? The genius of delegating, time management, and productivity? The master of communication, negotiating any terms imaginable?

Leadership has many aspects, and you can be many things, but you must remember that the power you hold is not yours — it was given to you — and that’s why you should honor your power giver’s goals above all else. Yet, remember also that you do not answer only to those who gave you the power — you answer to your conscience and your subordinates as well. Would you — hypothetically — be a welcome guest in their houses? When they return home after work, what do they tell their significant others about you? “That fucking bastard pissed me off again,” or “I wish more people were like that”?


## Architecture

### Homogeneity is not mandatory

It is generally good if your code is unified and homogenous, but this doesn’t have to always be required. Some parts of system are used more often than others, and it’s OK to have less important parts to be less polished than the core parts. It’s also generally optional and not required to refactor X part of the system, which is similar to the Y part you are refactoring now, but in this case you have to let your teammates know about your refactoring, so that they would be able to do the same thing when they will work with X part.


### In SOLID letters S and D are not mandatory

We do not code to follow rules, we use rules to aid us in coding. Sometimes disobeying rules may be more advantageous than following them. In my career I have experienced many times that enforcing S and D in SOLID was an overkill. If your class can do more than one thing, it doesn’t necessarily mean that it is low-quality, sometimes it is fine. It is even finer with D: it’s totally OK to be dependent on the MySQL client class instead of Database client class. We don’t need to create abstractions simply for the sake of creating abstractions, it’s permissible to do some hardcoding if it helps us to avoid increasing structural complexity of the system. Very few and only experienced developers can create good abstractions without having simultaneously at least TWO things which they are hiding under said abstraction. 


### Distributed monolith is not so bad

It is not inherently a bad thing to have multiple microservices in a distributed monolith architecture. There is not much difference between talking to your databases (which everyone does) and talking to your other microservices without truly-distributed architecture. Your architecture may become more complex, but the advantages are also here. You don’t always need a truly-distributed architecture. It is better to start with building distributed monolith and then rebuild it to a truly-distributed system than starting with a solid monolith and then rebuild it the same way. You get half of work done in advance without much expense.


## Python

### Avoid using comments in your code

I recommend avoiding comments in your code. I believe deeply that your code should be so simple, that the comments would become unnecessary. The problem is that no one trust comments. Whenever developer sees a comment, the first thing they always think about is whether this comment is outdated or not, because no one ever keeps them up to date, it’s the nature of things. I believe comments are acceptable when 1) you are explaining a calculated number, 2) you are explaining why you added a NOQA here, 3) your code is so complicated that you have to write the comment as final solution to make it at least somehow easier, 4) if your code generates some attributes and you want to make it easier to search project for them. Don’t forget that if you are writing a comment, never write it in a way that describes your code. The comment should always describe data or situation. Code is obvious, it’s not hard to understand that there is a for loop or something, it’s hard to understand why is it there and what’s inside of it. This issue is addressed in [doebus-django](https://github.com/vsevolod-skripnik/doebus-django) by using `flake8-fixme` and `flake8-todo`.

```Python
❌ 
# Here we have a loop to iterate over users
for user in users:
    user.reset_password()  # reset password

```
```Python
⭕️ MEMORY_SIZE = 3670016  # 1024 * 1024 * 3.5
```
```Python
⭕️ # noqa: SIM113 can't use enumerate here because of crazy yielding error
```
```Python
⭕️ data = producer.read()  # data is [{'id': int, 'price': int, 'timestamp': int}, ...]
```

```Python
⭕️ 
for attribute in ['username', 'password', 'bio', 'birthday']:
    data[f'user_{attribute}'] = None  # user_username, user_password, user_bio, user_birthday

```
    

### Split your code into actions and data

If you want to become a Python magician, the first thing you have to learn is to separate actions and data. By separating I mean clearly understanding when you are dealing with actions and when you are dealing with data. The following is the code which I consider to be a mess, because actions and data are mixed together:

```Python
❌ 
if value < 10:
    print('Small')
elif 10 <= value < 20:
    print('Medium')
else:
    print('Big')
    
```
In this code we have three different entities. The first entity is a group of conditions which we use in our `if` statement. The second entity is our need to have `if` here. The third entity is our need to print the result. This example is oversimplified to be concise, but in reality I’ve seen exactly the same problem with much more complicated things. It is not always reasonable to do the following, but I want to at least show you that the code could be rewritten like that: 

```Python
⭕️ 

NAME_CONDITIONS = {
    'Small': lambda value: value < 10,
    'Medium': lambda value: 10 <= value < 20,
    'Big': lambda value: 20 <= value,
}

for name, condition in NAME_CONDITIONS.items():
    if condition(value):
        break

print(value)

```
This refactoring may or may not be reasonable, but what matters is that in this piece of code 1) we see that our `if` statement is actually not a code, but data, 2) we would not have to write tons of `elif` statements if we will have tons of conditions, 3) we separate output from the action, which makes it easier to test. Generally speaking, every piece of your code ideally should always be either data or action, also there are different types of actions, such as calculation and output. This is related to abstraction layers and you have to master it.


### Structure your code as layered iterations

The second important thing for a Python magician is to see everything as iterations. Very often we deal with collections, such as lists, querysets and so on. Whenever we need to process one unit of collection, we always should split it into two abstraction layers: the first layer to iterate over collection, the second layer to process a single item. Processing a single item may at the same time be a place to iterate over another collection. This way the code becomes more isolated, less fragile, has better readability and is easier to test.

```Python
❌ 
class WordCatFinder:
    def __init__(self):
        self.articles = [
            {'id': 1, 'text': 'The cat is eating.'},
            {'id': 2, 'text': 'Looks like the cat is sleeping.'},
            {'id': 3, 'text': 'Dead is the cat.'},
        ]
    
    def run(self):
        records = []
        for article in self.articles:

            words = article['text'].split()            
            for i, word in enumerate(words):
                if word == 'cat':
                    break
            else:
                i = -1
            
            record = {
                'article_id': article['id'], 
                'word_position': i + 1,
            }
            records.append(record)
        return records
                    
```

```Python
⭕️ 
class WordCatFinder:
    def __init__(self):
        self.articles = [
            {'id': 1, 'text': 'The cat is eating.'},
            {'id': 2, 'text': 'Looks like the cat is sleeping.'},
            {'id': 3, 'text': 'Dead is the cat.'},
        ]

    def run(self):
        return self.get_records()

    def get_records(self):
        return [
            self.get_article_record(article)
            for article in self.articles
        ]

    def get_article_record(self, article):
        article_id = article['id']
        word_position = self.get_word_position(article['text'])
        return {
            'article_id': article['id'],
            'word_position': word_position + 1,
        }

    def get_word_position(self, text):
        words = text.split()
        for i, word in enumerate(words):
            if word == 'cat':
                return i
        else:
            return -1

```

### Keep your classes and methods brief

Try to keep your methods shorter than 20 lines of code. Having 20 to 40 is permissible in complex calculations, but having more than 40 is a bad practice which you should always avoid. There is nothing easier than splitting methods into submethods even without any logic refactoring, and you should always do it, because otherwise you will make it harder to read and harder to test, which ultimately makes this code prone to errors.


### Watch out for cyclomatic complexity

Your functions should have as few controlling statements (`if`, `for`, `while`) as possible. You should care about the cyclomatic complexity of your code, because the more execution branches the algorithm has, the more error-prone it becomes. It also becomes harder to read and harder to test, people tend to get lost in conditional switches and they may forget some scenarios. In [doebus-django](https://github.com/vsevolod-skripnik/doebus-django) this is addressed by using `flake8-cognitive-complexity`.


### Use variable prefixes and postfixes

If a variable has a datetime type, it should end in `_at` or `_until`. If a variable has a boolean type, it should start with `is_`, `are_`, `was_`, `has_` and so on. The reason is: if we call our variable `deleted`, it will be impossible to understand whether it is a date of deletion or a flag that it was deleted. If a variable is used to store the fact that we have to do some action, such as `Announcement(notify_subscribers=True)`, then it should have a `do_` prefix and be called something like `do_notify_subscribers`. In 9 out of 10 cases there simultaneously exist both method `notify_subscribers` and the variable which indicates that we have to do it. It’s easy to mix it up.

```
❌ updated, updated, update 
⭕️ updated_at, is_updated, do_update 
```

### On using for, to, in and from in variable names

If you have some users which you need to delete, you don’t call them `users_for_delete`, you call them `users_to_delete`. It may be obvious for native speakers, but for Russian-speaking developers it’s a common mistake. Also avoid using `from` and `in` if you can reverse the sentence without them.
```
❌ posts_for_add, users_to_deletion
⭕️ posts_to_add, users_for_deletion
```
```
❌ posts_in_feed
⭕️ feed_posts
```

```
❌ user_from_popularity_widget
⭕️ popularity_widget_user
```

### Avoid low-quality variable names

Avoid calling your variables `got`, `result`, `x`, `i`, `j` and so on. There are two reasons for that. First: it’s simply unclear what’s inside such variable. Second: it tempts to not understand your own code. When developer writes code, it’s their duty to know exactly what’s inside each variable. It always starts with little things. Today you call your variable `foobar`, in a week you will have bugs on production environment, I have seen it many times. Also I recommend to avoid including types in variable names, because they often change, but people don’t always change the name, so the name stays and brings confusion. One more rule to follow: if your class is `PostCreator`, you should call the instance `post_creator`, don’t call it simply `creator` or anything else, try make your code as predictable as possible. Also avoid shortening variable names for no reason (prefer `receive` over `recv`, `option` over `opt`, etc).

```
❌ got = membership_creator()
⭕️ membership = membership_creator()
```

```
❌ user_id_list
⭕️ user_ids
```

```
❌ creator = PostCreator()
⭕️ post_creator = PostCreator()
```


### Multiple lines separated by backslashes is a good thing

Make as little calls per line as possible, because it will reduce the cognitive complexity of your code and make it easier to read. People generally read from left to right, they lose focus from left to right. Try to keep the most important parts on left side, also utilize line breaks to avoid long lines.
```
❌ Post.objects.published().filter(published__gte=recently_period).values_list('id', flat=True)
⭕️ Post.objects \
    .published() \
    .filter(published__gte=recently_period) \
    .values_list('id', flat=True)

```

### Avoid magic numbers

Magic numbers are numbers which have unclear meaning. Consider example below. Seven what? Days, minutes, hours? Really confusing.

```
❌ Post.objects.published_recently(7)
⭕️ Post.objects.published_recently(days=7)
```


### Avoid Noun Adjunct violations

[Noun adjunct](https://en.wikipedia.org/wiki/Noun_adjunct) means that if we use noun before another noun, it usually has to be singular. For example if we have `cats` list and `names` list, and then we combine them, the result should be called `cat_names`, not `cats_names`. It’s the same thing as backend developers instead of backends delelopers. There are exceptions like sports companies, but in most cases the first noun should be singular.

```
❌ views_count
⭕️ view_count
```

```
❌ users_posts
⭕️ user_posts
```


### Learn standard library

Each Python developer should be aware of Python standard library utilities. Below is a list of the most important ones in my experience:

* `string` — common character lists, such as `ABCDEFGHIJKLMNOPQRSTUVWXYZ` and others

* `re` — regular expressions (`sub`, `findall`, etc)

* `datetime` — dates and times (`timedelta`, `date`, `time`, etc)

* `collections` — container data types (`namedtuple`, `deque`, `Counter`, `defaultdict`)

* `array` — python array implementation

* `copy` — copying objects

* `pprint` — printing objects with clean tabs and in a pretty fashion

* `enum` — enumerations

* `math` — some calculations like `sqrt`, `ceil`, `log`, etc

* `decimal` — better floats

* `fractions` — clear fraction representation

* `random` — random-related functions, like `choice`, `randint`, `randrange`, etc

* `statistics` — some statistic calculations like `mean`, `median`, etc

* `sqlite3` — lightweight SQL database

* `csv` — managing CSV files

* `hashlib` — hash function implementations for SHA, MD5 and so on

* `os` — files, directory traversing and other OS-related things

* `io` — working with data streams

* `time` — time-related things, such as `sleep`, `strftime`, etc

* `argparse` — CLI building tools

* `logging` — logging mechanisms

* `platform` — machine-specific underlying environment

* `threading` — multithreading tools for IO-bound tasks

* `multiprocessing` — multiprocessing tools for CPU-bound tasks

* `subprocess` — managing subprocesses

* `queue` — queue classes

* `asyncio` — concurrent code tools

* `json` — managing JSON files (`load`, `dump`, `loads`, `dumps`)

* `mimetypes` — work with MIME types (`guess_type`)

* `base64` — BASE enconding

* `uuid` — generate UUIDs

* `gettext` — internationalization tools

* `locale` — internationalization tools once again

* `typing` — type hints

* `pdb` — python debugger (`import pdb; pdb.set_trace()`)

* `venv` — managing virtual environments

* `timeit` — a tool to measure code performance

* `cProfile` — a tool which helps to understand which functions take how much time to run

* `sys` — some OS functions and tools (`exit`, `argv`, `path`)

* `builtins` — access to some underlying python language objects

* `dataclasses` — dataclasses, literally

* `contextlib` — a tool for building context managers

* `abc` — fancy base classes

* `importlib` — manual python imports


### Learn popular third-party tools

I believe the following tools are pretty useful in most projects:

* `requests` helps managing network requests

* `pytest` is a number one testing framework

* `click` helps to build beautiful CLIs




## Pytest

### Tests should always be simpler than the code they test

Whenever you write tests, always keep in mind that they have to be simpler than the code you are testing them with. If your test fails one day, the more complexity it has, the stronger will be your temptation to monkey-fix, skip or even remove it. This temptation is twice stronger if the test was written by someone else. It is your job as software engineer to keep all things as simple as possible, and tests especially.

```Python
❌ 
def test_send_mail():
    file = open('../../../../conf/config.json', 'r')
    config_string = file.read()
    file.close()
    config_dict = json.loads(config_string)
    username = config['username'] or os.getenv('MAIL_USERNAME')
    password = config['password'] or os.getenv('MAIL_PASSWORD')
    mailer = Mailer(username=username, password=password)
    mailer.configure(environment='testing')
    mailer.use_backend('stub')
    mail = mailer.get_mail(text='test mail text', emails=['test@foo.bar'])
    mail.send()
    assert mail.is_sent is True

```

```Python
⭕️ 
def test_send_mail(mail):
    mail.send()
    
    assert mail.is_sent is True
    
```


### Avoid class-based tests

When you start learning Python, you start with functions and only later learn about classes, the reason for that is classes being harder to understand. That’s exactly why you should also avoid them in tests: simplicity is everything in testing, "classy" tests bring temptation to make things more complicated — but in testing you don't need it. It is true that class-based tests have a scent of "structure", but they also have a scent of pain in the ass when you try to reuse `setUp` and `setUpClass` methods across different test-cases or try to remember what is already in self and what is not. In my career I have not yet seen a test of such complexity that it could not be clearly structured using fixtures and parametrizing. The simpler the test structure, the faster and easier it is to maintain it. By putting the necessary stuff into fixtures instead of `setUp` calls, you make it easier to access it. This allows to have great solutions, such as Single Source of Truth Fixtures, for example having only one instance of `Mail()` in most tests instead of repeating the same initializing call ten thousand times and fixing it ten thousand times later in different `setUp` calls when the class interfaces changes.

```Python
❌ 

class MailTestCase(unittest.TestCase):
    def setUp(self):
        self.mail = Mail(text='123', emails=['test@foo.bar']) 
    
    def test_send_mail(self):
        is_sent = self.mail.send()
        
        assert is_sent

```

```Python
⭕️ 

@pytest.fixture
def mail():
    return Mail(text='123', emails=['test@foo.bar'])


def test_send_mail(mail):
    mail.send()
    
    assert mail.is_sent is True

```

### Follow the AAA pattern

AAA pattern (Arrange, Act, Assert) states that each test should be divided into three blocks separated by blank lines: the arrange block (1-5 lines), the action block (always single line) and the assert block (any number of lines). This helps the viewer to clearly understand which lines are used as prep work and which line is being tested. The less lines arrange block has — the better. Arrange block also may be omitted if needed. Also in assert block you can put some lines which may not be asserts, but they prepare the data for being asserted, such as Django’s `refresh_from_db` calls.

```Python
❌ 
def test_publish_post(factory):
    post = factory.post()
    post_publisher = PostPublisher(post)
    post = post_publisher()
    assert post.is_published is True
    
```

```Python
⭕️ 
def test_publish_post(factory):
    post = factory.post()
    post_publisher = PostPublisher(post)
    
    post = post_publisher()
    
    assert post.is_published is True
    
```


```Python
⭕️ 
def test_send_mail(mail):
    mail.send()
    
    assert mail.is_sent is True
    
```

```Python
⭕️ 
def test_unpublish_post(post):
    post.unpublish()
    
    post.refresh_from_db()
    assert post.is_published is False
    
```

### Use symbolic asserts instead of functional asserts

I recommend using `assert` statements instead of classic `self.assertTrue` calls. When we see a word, we read it, and then we understand it, it takes some time, especially if we are tired. But when we see a group of symbols, such as `==` or `!=`, we understand them at a glance, because we see them so often in code. Another reason is that most syntax highlighting themes have a solid color for `assert` keyword, while something like `self.assertTrue` may not be even highlighted at all, or only `self` would be highlighted, even though it has the least value of the entire statement.

```Python
❌ 
self.assertTrue(mail.is_sent)
self.assertEqual(mailer.mail_count, 1)

```

```Python
⭕️ 
assert mail.is_sent
assert mailer.mail_count == 1

```

### Follow test naming template

When you write a test’s name, it’s usually the best if you follow this template: first you start with standard prefix `test_`, then you add snake-cased class name, then method name, then some feature or expectation, then `_if` followed by brief scenario description. Some parts may be omitted if we are testing the default good scenario. This template makes it easy to search for all tests where some exact class or method are being tested. Also if such test fails one day, you will clearly understand what are the expectations or use-case here. Remember to keep names the same way they are in code, for example do not put `not` between `is_` and some property if you are testing if something is false, also do not use `s` on the end of the verb, because it makes it harder to search project. If you are testing a scenario, then add a postfix starting with `_if`. Also note that if you feel like using `and` in your test name, probably you need two test or some data grouping. This template may not work in all cases and sometimes should be altered, but usually it works like a charm.

```Python
❌ test_is_sent
⭕️ test_mail_is_sent

```

```Python
❌ test_is_not_sent
⭕️ test_mail_is_sent_false

```

```Python
❌ test_bad_emails
⭕️ test_mailer_get_mail_if_bad_emails

```

```Python
❌ test_first_name_and_last_name
⭕️ test_first_name, test_last_name
⭕️ test_name_fields
```

### Follow a fixture naming template

Not always, but in most cases it is a good thing if your fixture name starts with object type and is followed by some properties or conditions. Naming fixtures in a predictable way helps to make them as readable as possible for other developers. There are exceptions to this, you may omit type in the name if it is generally obvious or will bring more confusion than clarity. In some situations you may create your own conventions, such as taking the first letter of user’s name or specifying the total amount of objects created by fixture and so on.

```Python
❌ post
❌ unpublished_unapproved_tomorrow_post
⭕️ post_unpublished_unapproved_tomorrow
```

```Python
❌ alice
⭕️ user_alice
```

```Python
❌ alice_and_bob
❌ user_alice_and_user_bob
⭕️ users_alice_bob
⭕️ users_ab
⭕️ users_2
```

```Python
⭕️ user_admin
⭕️ admin
```

### Use fixture name aliases

If your fixture names are long and hard to read, you may use a following trick to simplify it. It is also fine to have different names for the same fixture in different apps, for example if one app is aware of more details than the other.

```Python
⭕️ 
@pytest.fixture
def post_unpublished_unapproved_tomorrow(create_post):
    return create_post(is_published=False, is_approved=False, published_at='tomorrow')


@pytest.fixture
def post_bad(post_unpublished_unapproved_tomorrow):
    return post_unpublished_unapproved_tomorrow
```


### Treat your tests like you treat your code

Your tests should not be considered some kind of lab environment. The more you mock, the more you use prefixes likes `expected_` when asserting, the more you use general words like `result` without specifying what exactly is the result — the further your tests stand from your code, the further they stand from real life and the less value they have. This is not what you want. Your tests should be a source of truth on how to use your code in real world scenario. Whenever possible, avoid using such things as mocking, test data or unclear variables, asserts and statements. Your tests should be written in such a way that would leave no doubt that you had the full understanding of what you were testing and that you were testing the real life code and not some kind of imaginary make-believe testing mocked code. 

```Python
❌ 
def test_mail_is_sent(mocked_mail, expected_result):
    returned_result = mocked_mail.send()
    
    assert returned_result == expected_result

```

```Python
⭕️ 
def test_mail_is_sent(mail):
    is_sent = mocked_mail.send()
    
    assert is_sent is True

```

### Use fixture layering and chaining

If you have to create a bunch of fixtures that share some common things, try to layer the fixtures or chain them in such a way that would reduce the amount of repeating code. 

```Python
❌ 

@pytest.fixture
def user_alice():
    user = User(username='alice')
    user.save()
    return user


@pytest.fixture
def user_bob():
    user = User(username='bob')
    user.save()
    return user


@pytest.fixture
def users_2():
    user_alice = User(username='alice')
    user_bob = User(username='bob')
    return [user_alice, user_bob]

```

```Python
⭕️ 

@pytest.fixture
def create_user():
    def _create_user(username):
        user = User(username=username)
        user.save()
        return user
    return _create_user


@pytest.fixture
def user_alice(create_user):
    return create_user('alice')


@pytest.fixture
def user_bob(create_user):
    return create_user('bob')


@pytest.fixture
def users_2(user_alice, user_bob):
    return [user_alice, user_bob]

```

### Do not modify fixtures on the fly (including Django instances)

We often need to diverge from a common fixture version of model or some other data to add some custom flavor to it. In order to do so, you may feel temptation to simply set the corresponding attribute in arrange block. This makes your code fragile, because Django models are designed in a such way that if you set an attribute which is not a field, there will be no error. For example, if the model has an `author` field, but I type it as `post.athor = user`, the code won’t break, but this may cause some crazy invisible bugs. And there is more to that. By simply setting attributes on the fly we think of fixture as of piece of data and not as of domain logic entity. If creating a Post instance also causes some author field processing via `PostCreator` service, it simply won't happen if you set the attribute manually. As the result, the tests are testing artificial functionality ignoring some crucial business domain logic. In any fixture no kind of data (including Django model instance) should be in any way modified from the moment of entering the test until the moment of action line. Test data should always reach action line in the same form that it was when the test started.


```Python
❌
def test_post_of_user(post, user):
    post.author = user
    post.save()

    assert user.authored_posts.first() == post

```
```Python
❌
@pytest.fixture
def post_of_user(user):
    post = factory.post()
    post.author = user
    post.save()
    return post

```
```Python

⭕️
@pytest.fixture
def post_of_user(user):
    return factory.post(author=user)

```


### Use pytest parametrizing

Parametrizing is a mechanism of running your test multiple times, each time providing it with different input values. This allows you to easily increase amount of tests in your project, while typing less. Also it helps to better identify which input data caused the test to fail. 

```Python
❌ 
def test_post_strip_text():
    for text, stripped_text in [['42   ', '42'], [' 42 ', '42'], ['   42', '42']]:
        post = Post(text=text)

        assert post.text == stripped_text    

```

```Python
⭕️ 
@pytest.mark.parametrize(
    ('text', 'stripped_text'),
    [
        ('42  ', '42'),
        (' 42 ', '42'),
        ('  42', '42'),        
    ],
)
def test_post_strip_text(text, stripped_text):
    post = Post(text=text)
    
    assert post.text == stripped_text    

```

### Use time-mocking packages

A common scenario: you created an object and you have to test if the `created_at` is correct, but whenever you run this test, the `timezone.now()` gets a different value. What shall you do? Avoid it? Mock it? As with pretty much anything in development, people have already addressed it. In this case the answer is to use time-mocking packages, such are `freezegun` and `time_machine`. There may be more, but the idea is same: they allow you to manipulate the system time in tests, switch it using `with` statements or travel timedeltas to emulate time passing. In [doebus-django](https://github.com/vsevolod-skripnik/doebus-django) this is achieved by using `freezegun`.

```Python
⭕️ 
@pytest.mark.freeze_time('2001-01-01')
def test_post_published_at():
    post = Post()

    post.publish()
    
    assert post.published_at == '2001-01-01T00:00:00Z'    

```

### Learn pytest.ini and command-line options

Pytest has a configuration file called `pytest.ini` which you can use to filter warnings, alter test discovery, skip some tests, configure environment variables and tweak some other behavior, such pytest modules. I recommend diving into its documentation to understand what is at least theoretically possible to achieve by simple means. My favorites are for example `-x` and `--lf` command-line options, which respectively mean to exit on first failed test and only run the tests which failed during the previous run.


### Testing layers

Your application should have a defined testing structure, which should consist of multiple levels. The following is an example for modern Django:

1. API Layer. Every API endpoint should have some tests and ensure that 1) URL exists, 2) no unauthorized access is possible, 3) a correct business logic container is being called, 4) request and response data formats are valid, 5) request validation works fine.

2. Serializer Layer. Generally, you don’t need to test your serializers outside of API tests. The absence and manual forging of `request` and `context` make serializer tests way too synthetic. Your tests should be used to validate real code instead and of code created for tests. But sometimes it is necessary to test serializers, for example if they have some validation which is reused across multiple API endpoints, or if they have some hard-to-test low-level methods.

3. Viewset Layer. There is no way to properly test a viewset. If you need to test your viewset, try to put your viewset code into different functions and test them instead. Otherwise you should test your viewset in your API layer.

4. RPC Layer. RPC tests are similar to API tests and should check 1) input and output format, 2) method naming, 3) ensure proper business logic containers are called.

5. Business Logic Layer. A “Processor” or “Service” is a class dedicated to an atomic business logic task. You should have many tests for each processor. A lot of unittesting should be happening here, other places should be mostly functional.

6. Model Layer. Sometimes you have to override the `.save()` method or put some logic into your model. Such functionality should be tested on model layer.


## Django

### Your business domain logic should be in services

Historically people used models to store business domain logic, but in my experience it’s better to store it in services, because they are easier to test and maintain. Service is a small class which inherits from BaseService and does some piece of business logic. Typical services include `PostCreator`, `UserImporter`, `MembershipUpdater` and so on. The most important part of service is `act` method, it should be overriden and store service code inside of it. If validation is needed, override `validate` method. Remember that in 95% of cases the service needs to return some kind of object or objects, usually the ones which were processed by it. More services is better, smaller services are better. This is addressed via `BaseService` in [doebus-django.](https://github.com/vsevolod-skripnik/doebus-django)

```Python
❌
class Post(DefaultModel):
    ...
    
    def publish(self):
        self.is_published = True
        self.published_at = now()
        self.save()
```

```Python
⭕️
class PostPublisher(BaseService):
    def __init__(self, post):
        self.post = post
    
    def act(self):
        self.post.is_published = True
        self.post.published_at = now()
        self.post.save()
        return self.post

```


### Avoid generated migration names

Your migrations should be named manually. Sometimes you have to do some investigating in your migrations, and it is easier to do so if they are named. Also, if you don’t have a name generation each time you run `makemigrations`, you are thus forced to look into each of generated migrations per app and validate its contents, which is a good thing. In [doebus-django](https://github.com/vsevolod-skripnik/django-doebus) this issue is addressed by overriding standard `makemigrations` command.


### Avoid having large settings file, avoid having multiple settings files too 

One of the oldest anti-patterns in Django community is using dev and prod settings. I consider it a harmful practice, because having multiple settings files forces you to either violate DRY, or keep in mind the mechanism of inheritance, which sometimes gets messy. The issue becomes even worse if your CI is not straightforward too, there arises a risk of losing track of what is defined where. The best solution out there is to use a single settings file, but separated into different section-files. Dev and prod differences should be addressed by tweaking variables with switches and environment conditions. This is present in [doebus-django.](https://github.com/vsevolod-skripnik/doebus-django)


### Avoid filtering querysets outside of home app

If according to business domain logic a post is considered public if it is published, approved by moderators and so on, all these conditions should be stored in a single well-known place, usually it’s a manager method in home app. It is a bad practice to disperse this logic across other apps, because this will force us to keep all the places in mind and don't forget to fix them too whenever we have to add a new condition. 

```Python
❌ Dispersing across apps:

# List all posts on main page queryset
public_posts = Post.objects.filter(is_published=True, is_restricted=False)

# User posts queryset in users app
authored_posts = Post.objects.filter(is_published=True, is_restricted=False, author=user)

# Sitemap posts queryset in sitemaps app
sitemap_posts = Post.objects.filter(is_published=True, is_restricted=False, published_at__gt=week_ago)

```

```Python
⭕️ Centralized storing:

# Post model manager queryset
class PostQuerySet(DefaultQuerySet):
    def public(self):
        return self.filter(is_published=True, is_restricted=False)


# List all posts on main page queryset
public_posts = Post.objects.public()

# User posts queryset in users app
authored_posts = Post.objects.public().filter(author=user)

# Sitemap posts queryset in sitemaps app
sitemap_posts = Post.objects.public().filter(published_at__gt=week_ago)

```

### Use starter templates

It is generally beneficial to use app and project templates, because it 1) speeds up development, 2) helps new developers to better understand what is a bare-minimum, 3) keep apps and projects in a more unified structure. For apps use `--template` option of `startapp` command, for projects use `cookiecutter`. This is addressed by overriding `startapp` command in [doebus-django.](https://github.com/vsevolod-skripnik/doebus-django)
