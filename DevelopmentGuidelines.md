

The thing I love the most about Python has to do with it not having braces. It's not the lack of braces, though... but the in-built enforcement of common indentation, as a result of not having braces.

The idea is to write code that blends in with the code around it; be consistent, basically, in your style and structure; to morph seamlessly with the code around you. Python enforces this at the interpreter level, but this can be ensured with good practices, and if necessary, with linting tools.

As a pattern, though, it's quite simple, and it can be used universally, regardless of the language.


## Readability
Code should be readable; after all, it is read more often than it is written. It should be clear, concise, and easy to understand. Variables, classes, methods, etc, should be named in a meaningful and intuitive manner.

### CamelCase and under_scores: know when to use each.
 * ClassNames
 * classMethods
 * class_properties

### Spacing
* Use 4 spaces to indent when possible, unless more are necessary (ex: linux kernel requires 8 space tab stops) or when hard tabs are necessary (ex: makefiles)

### {}[]\n\r
* Braces/brackets do not belong on new lines.
* But new lines belong after conditionals, functions, and other blocks.

### Breaking the rules
Some libraries break these conventions, we don't live in a utopia, after all... so this is where consistency comes into play. Rules are meant to be broken, but you have to know when to break them, and when not to.

### Proper tool for the job
Your IDE should be capable of auto-detecting the tab width of the file that you're editing, and when you hit tab, it uses the proper number of space characters.

There is no universal standard for spacing or formatting, it depends on the environment, and the team.

For example:

> (Popular with the Linux kernel.)
> > Tabs are eight columns wide. Each indentation level is one tab. 

> (Popular with Windows developers using Visual Studio.)
> > Tabs are four columns wide. Each indentation level is one tab. 

> (Popular with Java programmers.)
> > Each indentation level is four spaces. Tabs are not used. 

### Whitespace, by example.

```python
spam(ham[1], {eggs: 2})       # YES
spam( ham[ 1 ], { eggs: 2 } ) # NO

spam(1)  # YES
spam (1) # NO

dct['key'] = lst[index]   # YES
dct ['key'] = lst [index] # NO
```

```php
// YES
function test(){
    if{
        //....
    }
}


// NO
function test(){
    if
    {
        //...
    }
}

// NO
function test(){
    if{
        //...
      }
}

```

A common "No" example is this:
```python
x             = 1
y             = 2
long_variable = 3
```

But, I often write code like this, as I think it is much easier to read/skim:

```python

class TaskWorker(Thread):
    def __init__(self, app=None, queue_key=None, rv_ttl=None, redis=None, worker_name=None, thread_name=None, debug=None):
        Thread.__init__(self, name=thread_name)
        self.daemon         = True
        self.queue_key      = queue_key
        self.rv_ttl         = rv_ttl or 900
        self.redis          = redis or create_redis()
        self.worker_name    = worker_name or (thread_name if worker_name is None else None)
        self.debug          = app.debug if (debug is None and app) else (debug or False)
        self._worker_prefix = (self.worker_name + ': ') if self.worker_name else ''

```

But one must be careful with this. If there is a really long variable it makes it more difficult to read, you have to be careful, for example, this:

```python
@audit
class Client(db.Document):
    name                 = db.StringField(required=True, max_length=50)
    address              = db.StringField()
    city                 = db.StringField()
    state                = db.ReferenceField('State', required=True)
    zip                  = db.IntField()
    phone                = db.StringField()
    designated_operator  = db.StringField()
    inspection_types     = db.ListField(db.EmbeddedDocumentField(InspectionTypeTemplate, required=True))
    email_recipients     = db.ListField(db.EmailField())
    report_logo          = db.ImageField()
    sg_id                = db.IntField() #todo: make this unique after resolving missing data
```

...is not as readable as this:

```python

@audit
class Client(db.Document):
    name     = db.StringField(required=True, max_length=50)
    address  = db.StringField()
    city     = db.StringField()
    state    = db.ReferenceField('State', required=True)
    zip      = db.IntField()
    phone    = db.StringField()

    designated_operator  = db.StringField()

    inspection_types  = db.ListField(db.EmbeddedDocumentField(InspectionTypeTemplate, required=True))
    email_recipients  = db.ListField(db.EmailField())

    report_logo  = db.ImageField()
    sg_id        = db.IntField() #todo: make this unique after resolving missing data

```

And I'm guilty of writing things like this sometimes, too:

```python
class ATGInfo(db.EmbeddedDocument):
    atg_present       = db.BooleanField( verbose_name='ATG Present?')
    atg_type          = db.StringField(  verbose_name='ATG Type')
    num_dispensers    = db.IntField(     verbose_name='# Dispensers')
    dispenser_type    = db.StringField(  verbose_name='Dispenser Type', choices=['multiple product', 'single product'])
    stage_2_recovery  = db.BooleanField( verbose_name='Stage 2 Recovery')
```

It's frowned upon by some ([PEP8 says this is bad](https://www.python.org/dev/peps/pep-0008/#pet-peeves)), but it makes certain code much more readable, in context.

## Avoid Complexity

Avoid complex methods, try to write easily decipherable chunks that can be easily comprehended, and tested as necessary; but don't create a new method for everything. Find a balance between massive spaghetti functions with tons of conditionals with a pile of one-off functions vs doing everything in a single method.

Obviously, consistency and complexity will deviate at various levels, between modules, and such, but at a low level, in a method or function, or in a class, consistency and simplicity are critical. It's important at higher levels, like with in a project, as well, but that is beyond the scope of this document.


## Version Control

### Commit Messages
Leave meaningful commit messages. If the project is integrated with a task/issue tracker, be sure to reference any issues or cards identifiers in the commit message.

### Stay up to date
If you're working on a project with others, it's always a good idea to pull down the most recent changes from upstream, if any exist, before you start working.

### Keep everyone else up to date
Keep the team up-to-date by committing and pushing often, as meaningful progress is made. Committing infrequently is not adequate if you're working on many things with a team involved and you're completing multiple features, especially if there is overlap in code coverage of features. If you don't commit often at regular intervals as progress is made, there will inevitably be merge conflicts. 

### Test your code!
However, do not push before your changes are ready to be pushed! If you have not even sanity checked your code, and it doesn't even compile or parse before you send a pull request, that is a bad sign and will not be tolerated for long. It's one thing to commit often if you're making lots of meaningful changes and true progress, it's another thing entirely to commit often if you're sending pull requests for untested and incomplete features. Test your code, pull the branch that you're requesting to merge into to be sure your merge works before you request it!

### Be precise
Be explicit in what files you add to a changeset, do not just `git add *` or `git add .` without doing a `git status` to check what you're doing. Always be sure you know what you're doing, don't just sling crap at the interpreter and expect good results. Be patient, concise, and explicit.

### Feature Branches and Pull Requests
All changes related to a feature should be performed in a single branch, and a pull request should be made to staging when it is ready to be merged. This is so changes can be reverted and rolled back easily, later, if needed. 

### Merge Strategy
Do not merge between feature branches, and do not merge from staging to feature branches. 

If it's not ready to go live yet, put it in its own branch, and don't litter master, staging, or other feature branches.

Don't leave old commented code laying around, unless it's perhaps an old implementation that you refactored, and it's relevant to explain in comments why something is the way it is. We have version control for a reason, there's no need to comment things to save them. If you change something, leave a descriptive commit message, and move on. If you have something half-completed or are just tinkering with an idea, either use a stash, or use another local-only branch.


### Rebase?
_**Do not use git rebase**_, unless you know what you are doing. 

Git rebase should only be used on your own code, with-in your own feature branches. DO NOT USE GIT REBASE on code that other developers are also working on. For example, it's commonly advocated on StackOverflow to use git rebase instead of git merge to allow fast-forward merges. This is a really really really bad idea. Don't do this, ever, unless you're the only one working on this code, and you understand clearly what is happening. If you have to ask, don't do it.


### Housekeeping
I like to use `todo` comments and `fixme`. If you pass by something in a code dive that needs investigating, but it's unrelated to what you're doing, and you'd like to avoid yak shaving, it's a good idea to jot down a comment and move on. If you prepend it with a commonly agreed upon string, like todo or fixme, then later you or another dev can `git grep` the code base for all todos or fixme comments.

**Example:**
```
backend/namespaces/user.py
30:                #todo: log this

backend/report.py
79:            #todo: move path to config
80:            #todo: setup tests for file paths, run on launch and fail early if broken
98:#todo: break this out into multiple methods so we can generate only one type of report PDF if desired
179:    #todo: delete report files too?

backend/reports/hertz-fuel-inspection/__init__.py
782:                    #todo: reenable when app supports decimal prompts
796:                        #todo: why combining base and condition identifiers here?

backend/reports/standard_fuel_inspection.py
82:                    #todo, log this?
134:                    #todo: log this?
412:                                                    #todo: set text
456:                                    #todo: fallback for fields that no longer exist
520:                                            #todo: set text
```

As an aside, something that's visible here, and I feel is worth pointing out, just a little nice thing about clean and common indention: seeing the indentation level gives an idea of complexity and scope.

Similarly, if you are in the middle of something, and have to step away for a minute, or need to move on to something else, you can leave a comment like `stepin: blah blah blah` along with a description of what's on your mind at the moment, and list details of what all is needed to step back into scope if you're coming back to your current position with a fresh set of eyes.

## Safe Security Practices

**Use safe and trusted libraries.** 

For Python, we use libraries like [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/), [Flask-Security](https://pythonhosted.org/Flask-Security/), [Flask-WTF](https://flask-wtf.readthedocs.io/en/stable/), and [itsdangerous](https://pythonhosted.org/itsdangerous/) which provide various levels of protection against various types of attacks like CSRF, XSS, etc...

_todo: fill in JS/PHP security libraries here._

Always sanitize and escape user input, even if trusted. Similarly, also be sure that output is escaped as well. Most libraries handles this these days, but you can't be to careful. Stay vigilant, and never forget to sanitize input.

Avoid eval, subprocess, exec, subqueries, etc... unless it's necessary and you know what you're doing.

_todo: fill in more best practices_
