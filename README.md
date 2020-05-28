# Overview

Basic examples of apps with feature toggles.

If you havent' already, take a look at the Feature Toggle Primer: https://gist.github.com/JAMSUPREME/8c4450b584c8a72170f2be84b458b3ff

# Startup

I recommend opening up two shells, one for your ruby app and the other for your unleash resources.
```
# Shell 1
# cd ~/Github/toggle-stuff/rails-app
ruby s
```

```
# Shell 2
# cd ~/Github/toggle-stuff/unleash-docker
docker-compose up
```

# Opinions about unleash

- No out-of-the-box scheduling support
- "Enabled" by default (when creating in UI, though you can disable)
- No API in the client for creating toggles (you could probably build a wrapper or write SQL)

# Testing

We'll do a pretty basic setup where each scenario gets its own block. There are different ways to organize your tests depending on how you're changing the code, but usually adding another scenario in the existing scope is OK.

In Ruby, I recommend using `Mocha` for mocking/stubbing because I find it to be the easiest to read.


# Questions

- How to manage the toggles without using a lot of if/else blocks? - Factory, if else block, or custom abstracitons (facades and such)
- How do we remove old toggles and avoid tech debt? - Naming convention is important for toggles, and usually a CTRL+F is good enough. It is also a good idea to make user stories to clean up the toggle once you've put it in prod.

# Creating and maintaining toggles?

- Ideally, you have a wrapper around the creation and then some tooling to "migrate" up to a particular point.
```
# hello_restructure insert
INSERT INTO features "hello_restructure"
```
```
# hello_restructure rollback
DELETE FROM features where name = "hello_restructure
```

```
# imaginary tooling library
# this would check if was accessed, etc.
UNLEASH.permanently_deactivate("hello_restructure")
```

If you were curious, you could read into these for extending unleash:
- https://github.com/Unleash/unleash/blob/master/docs/database-schema.md
- https://github.com/Unleash/unleash-client-ruby/blob/master/lib/unleash/client.rb

# Code/toggle mismatch?

```
# angular components (ng_components) library v1, v2, v3
v2 -> "hello_restructure"
```

```
# Gemfile (ruby dependency tree)
ng_components v2
```

Bad scenario: You upgraded to v3, cleaned up the "hello" toggle, then had to revert to v2 and stuff is broken