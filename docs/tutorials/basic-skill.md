# Creating a Basic Skill
We will create a basic skill that makes opsdroid answer the text "how are you". This skill will be very similar to the [hello skill](docs/extending/skills/#hello-world) found in the documentation.
The video tutorial for creating your own skill is also available [here](https://www.youtube.com/watch?v=gk7JN4e5l_4&index=3&list=PLViQCHlMbEq5nZL6VNrUxu--Of1uCpflq)

*If you need help or if you are unsure about something join our* [gitter](https://gitter.im/opsdroid/) *and ask away! We are more than happy to help you.*

## The Skill folder
Opsdroid skills are located inside a folder with the name `skill-<skillname>`. Inside this folder you should have at least these files:

- `LICENSE`
- `README.md`
- `__init__.py`

You will write all of your python functions in the `__init__.py` file, but you can also include any other helpful files inside your skill folder.

## Building the Skill
We are now ready to start writing the 'how are you' skill. We will be using opsdroid regex matcher to write our function. The first thing to do inside our `__init__.py` will be importing the `Skill` class from opsdroid to inherit from.

#### Importing the skill class

```python
from opsdroid.skill import Skill
```

#### Importing Regex Matcher

We also need a matcher to use to connect a method of our skill class to a phrase or sentence that the user will say.

```python
from opsdroid.matchers import match_regex
```

All the matchers available in opsdroid can be imported from `opsdroid.matchers`.

#### Decorators
A matcher is meant to be used as a function decorator. The decorator allows opsdroid to understand the function, so we need to use this decorator and use the regex, that we wish opsdroid to react to.

```python
from opsdroid.skill import Skill
from opsdroid.matchers import match_regex

class MySkill(Skill):

    @match_regex('how are you?')
```

The [regex matcher](/matchers/regex.md) takes a regular expression and searches for it on every message sent by a user. So if the user types `how are you?` opsdroid will trigger the function underneath the `match_regex` decorator.

_Note: Opsdroid won't trigger with the text `how are you` because the question mark is missing._

#### Functions
Skills in opsdroid are just Python methods on a class. As seen before, a decorator and a method together will allow opsdroid to do pretty much everything that you can imagine.

Let's write our function, so opsdroid knows what to trigger when a user writes the words `how are you?`

```python
from opsdroid.skill import Skill
from opsdroid.matchers import match_regex

class MySkill(Skill):

    @match_regex('how are you?')
    async def how_are_you(self, message):
        pass
```

Opsdroid is build with asyncio. That means every function that you wish opsdroid to react to, should be an asynchronous function.

_Note: that every function will take these two parameters: `self`, `message`._

#### Opsdroid Answer
So far our opsdroid can match something that a user said and can trigger a skill. But nothing will happen yet, let's make opsdroid answer to the text, with a message of its own.

```python
from opsdroid.skill import Skill
from opsdroid.matchers import match_regex

class MySkill(Skill):

    @match_regex('how are you?')
    async def how_are_you(self, message):
        await message.respond('Good thanks! My load average is 0.2, 0.1, 0.1.')
```

Our skill is done and opsdroid will be able to answer like in the [Opsdroid main page](https://opsdroid.github.io). The next thing that is left to do is to add the skill to the opsdroid configuration file.


#### Adding the Skill To Configuration
For the sake of simplicity we will assume the following:
- The configuration file is located at: `~/.opsdroid/configuration.yaml`
- Your skill is located at: `~/documents/skill-howareyou`

Open your `configuration.yaml` file and under the skills add the name and path of your skill like such:

```yaml
skills:
  - name: how-are-you
    path: /Users/username/documents/skill-howareyou
```

As mentioned in the [introduction](introduction.md/), the indentation and the use of spaces instead of tabs is very important. If you have any issues when running opsdroid check both of this things, it might be a problem with a space.

