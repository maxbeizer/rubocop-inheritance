# Rubocop Inheritance Example

related issue: https://github.com/bbatsov/rubocop/issues/4459

Using [Rubocop](https://github.com/bbatsov/rubocop), version `0.49.1`, this repo
tests that sub-folders can have `.rubocop.yml` files that override behavior of
the top-level config

## What I'm seeing

When I run rubocop against this project, I would expect the
`some_dir/.rubocop.yml` to disable all cops on `some_dir/my_other_file.rb`

I would expect to see no errors since `my_other_file.rb` should be excluded.
Instead, I see:

```
$ rubocop
Inspecting 3 files
..E

Offenses:

some_dir/my_other_file.rb:2:17: C: Freeze mutable objects assigned to constants.
  MY_CONSTANT = 'MY CONSTANT'
                ^^^^^^^^^^^^^
some_dir/my_other_file.rb:5:5: E: Prefer single-quoted strings when you don't need string interpolation or special symbols.
    "double quotes :scream:"
    ^^^^^^^^^^^^^^^^^^^^^^^^

3 files inspected, 2 offenses detected
```

So, `some_dir/.rubocop.yml` 's config is clearly not excluding that file:

```yml
inherit_from: ../.rubocop.yml

AllCops:
  Exclude:
    - 'some_dir/my_other_file.rb'
```
