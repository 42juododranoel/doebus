# Vsevolod Skripnik’s Developer Experience Guidelines

This is a list of my personal, really opinionated rules, which I believe to be beneficial and important. Each and every one of them I experienced myself many times and I am 100% sure that they bring lots value if utilized correctly. This list is not final, it gets extended as I get new experiences.

| Table of Contents |
| ----------------- |
| [Developer Experience](#developer-experience) |
| [Architecture](#architecture) |
| [Python](#python) |
| [Pytest](#pytest) |
| [Django](#django) |


## Developer Experience

### Ownership, responsibility and home feeling

It is crucial for developers to feel some degree of ownership and responsibility for their repository. Backend team should have their backend repositories only for themselves, same with frontend, infrastructure and other teams. It is discouraged to mix teams together in one repository. Developers should feel at home in their repository, because otherwise the dormitory effect kicks in: if something belongs to everybody, then it belongs to nobody, and nobody is responsible for housekeeping in it. People generally like to care about quality and keep things clean and tidy, but only if they feel connection to the place. It is also a good idea to have a home repository for each developer where they are allowed to make things their way (to some limited degree). It helps to reduce number of arguments and help people see that there are good practices in any styles and architectures.


### Follow Test Driven Development

I don’t remember who said it, but code without tests is legacy by design. There are tons of pros to writing tests: 1) easier to hire high-quality developers, 2) no recurring bugs, 3) accurate estimation of time requirements for tasks, 4) saving time and money on manual testing, 5) higher developer morale, 6) being able to perform architecture adjustments which would be unfeasible without tests, 7) new developers learn project faster, 8) bigger trust between developers, 9) less stress for developers when sotware malfunctions. Avoiding using tests in modern development is the same as avoiding using cars in logistics, instead relying on ye olde horses. There exists a popular misconception that writing and maintaining tests takes longer time than working without them, but this is based on a wrong assumption that it takes the same amount of time to implement the code with and without tests. In reality writing tests speeds up coding by making it easier to run this code under different conditions in a friendly environment. There doesn’t exist a single reason to avoid writing tests.


### Use linters

Linters are automated tools which highlight potential problems in your code. The more linters — the better. Linters should be run on each build before tests. It is recommended to configure your IDE to undo its custom linting and follow the project linting rules. A PR can’t be merged if it has any linting errors. It is also a good idea to force unified code formatting using automated tool like Black.


### Bulletproof deployments

Your deployments should be predictable and simple. Your developers should always 1) know exactly which version is running on which server, 2) have a simple interface to deploy their code, 3) have easy access to deploy logs in case if something fails. Your deployments should always be automated and ideally should be able to downgrade themselves automatically. 


### Task code review practice

Each PR should be reviewed by one teammate of same or higher seniority level. This rule is not required for seniors and tech lead, but is mandatory for middle, junior and intern developers. There are multiple benefits of code review: 1) having one more pair of eyes looking at the solution helps to bring quality to it, 2) developers learn from each other, which makes them more productive and create more value for the project in the long run, 3) it helps to avoid bugs and errors since all humans sometimes make mistakes.


### Task product review practice

Each task should be validated by someone from product team, who either created the task, or is responsible for the task’s domain. Each frontend task should also be reviewed by a designer. The goal of product review is to make sure that developer’s solution is considered good and solving original problem or bringing necessary value to the product. The goal of design review is to make sure that UI and UX are solid and high-quality.


### QA testing is overrated

You generally don’t need QA engineers, your developers can do the QA part themselves. However, this requires a high degree of responsibility. You can only achieve it in small teams with high morale and quality developer experience. If you have QA engineers, they should not be held responsible for mistakes of developers. Whatever the workflow is, it is ultimately developer’s responsibility to make quality product.    


### You don’t need coverage metrics

In my experience, coverage metrics are make-believe, having a 100% code coverage doesn’t equate to having 100% product use cases coverage. Some code can’t or doesn’t even need to be covered at all. Having a coverage level requirement often forces developers into thinking less about the quality of their work and more about tricking the coverage system into thinking that the work has high enough quality.


### Multiple developer environments are overrated

You don’t necessarily need local, dev, stage and prod environments. Having local and prod is enough. The more environments you have, the more complexity, the less efficient your work. Having dev server may aid in coding and testing, but the dev should never be treated the same way you treat your production environment. It shouldn’t be something “sentient”, it should be easy to maintain and recreate if something gets broken. If you think that your infrastructure is too complex to be run on local machine, it doesn’t necessarily mean that you have to invest into a staging environment, maybe you have to invest in better mocking or isolating. The best strategy to strive for is to have your local machine and production as similar as possible.


### Distributed monolith is not so bad

It is not inherently a bad thing to have multiple microservices in a distributed monolith architecture. There is not much difference between talking to your databases (which everyone does) and talking to your other microservices without truly-distributed architecture. Your architecture may become more complex, but the advantages are also here. You don’t always need a truly-distributed architecture. It is better to start with building distributed monolith and then rebuild it to a truly-distributed system than starting with a solid monolith and then rebuild it the same way. You get half of work done in advance without much expense.


### Some degree of f* around is OK

Your developers should be allowed to do some f* around (provided that it’s not harmful). It is permissible to break your own rules sometimes, for example to violate code style conventions in some places. If you always do things this way or that way, you may miss some important discoveries. Sometimes the only way to find a solution for a problem is to f* around it with utmost disrespect for any rules.


## Architecture

### Homogeneity is not mandatory

It is generally good if your code is unified and homogenous, but this doesn’t have to always be required. Some parts of system are used more often than others, and it’s OK to have less important parts to be less polished than the core parts. It’s also generally optional and not required to refactor X part of the system, which is similar to the Y part you are refactoring now, but in this case you have to let your teammates know about your refactoring, so that they would be able to do the same thing when they will work with X part.


### In SOLID letters S and D are not mandatory

We do not code to follow rules, we use rules to aid us in coding. Sometimes disobeying rules may be more advantageous than following them. In my career I have experienced many times that enforcing S and D in SOLID was an overkill. If your class can do more than one thing, it doesn’t necessarily mean that it is low-quality, sometimes it is fine. It is even finer with D: it’s totally OK to be dependent on the MySQL client class instead of Database client class. We don’t need to create abstractions simply for the sake of creating abstractions, it’s permissible to do some hardcoding if it helps us to avoid increasing structural complexity of the system. Very few and only experienced developers can create good abstractions without having simultaneously at least TWO things which they are hiding under said abstraction. 


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
