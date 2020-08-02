This is an implementation of Kate Compton's [Tracery](https://github.com/galaxykate/tracery) language in pure lua. The library takes a recursive approach to managing generative state, and returns any un-popped rules that have been pushed to the stack during the course of generation.

## Basic usage

```lua
local tracery = require "tracery"

local testGrammar = {
  origin = "#group.a# of #animal.s# #number#",
  animal = {"zebra", "osprey", "rhinoceros", "cassowary", "donkey", "eel part"},
  group = {"group", "herd", "flock", "murder"},
  number = function() return 10 end,
}

local grammar = tracery.createGrammar(testGrammar)
grammer:addModifiers(tracery.baseEngModifiers)
local text, data = grammar:flatten("You see #origin#")

```

# API

## tracery.createGrammar(tbl)
Creates a [Grammar] object, using the optional `tbl` as the base rule set. This rule set is immutable.

# Grammar
The following are the methods exposed by a Grammar object.

## grammar:flatten(text) -> text, data
Resolves any rules in a piece of text and returns any pushed rules from the process in a table.

## grammar:generate(symbol) -> text, data
A shorthand for `grammar:flatten("#symbol#")`. Useful if you want to generate from a single symbol and not a more complex piece of text.

## grammar:addRule(symbol, value)
Pushes a rule onto the context of the grammar (destructive push).

## grammar:delRule(symbol)
Removes a rule from the context.

## grammar:pushRules(rules)
Pushes a new context table onto the grammar stack (non destructive push).

## grammar:popRules()
Pops off a context stack and restores the previous context stack.

## grammar:addModfiers(modifiers)
Adds the given modifiers table to the gramar. Modifier tables are in the form `
{modifier_name = function(text)}`.

## grammar:clearState()
Deletes the un-popped rules from the grammar's last generation.

# Modifiers
The same modifiers from Kate Compton's original tracery are accessible as `tracery.baseEngModifiers`.

# Changes from the reference implementation
## Unintentional differences/bugs
- (See issue #1) POP's inside of rules don't affect higher pushes.

Please raise an issue if you find any more!

## Function rules
Grammars can contain functions as the value for a symbol. This function will be called with no arguments, and will have their return values inserted into the output text.

```lua
>>> grammar = createGrammar({
      origin = "Your lucky number is: #number#",
      number = function()
        return math.random(42)
      end
    })

>>> text, _ = grammar:generate("origin")
>>> print(text)
Your lucky number is: 34

```

## Upcoming extensions to the language
- Allow repetition of rules by a given number 
- Allow custom weighting rules for array rules

# Copyright
- lua-tracery is Copyright Aldous Rice-Leech 2020
- Tracery is Copyright Kate Compton 2015

lua-tracery inherits the Apache Public License from Tracery
See [LICENSE.md](/LICENSE.md) for more details
