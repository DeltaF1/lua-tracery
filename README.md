This is an implementation of Kate Compton's [Tracery](https://github.com/galaxykate/tracery) language in pure lua. The library takes a recursive approach to managing generative state, and returns any un-popped rules that have been pushed to the stack during the course of generation.

## Basic usage
This OOP interface is not guaranteed to remain stable, I have yet to determine what paradigm this library should follow.

```lua
local Grammar = require("tracery").Grammar

testGrammar = {
  origin = "#group.a.capitalize# of #animal.s# #number#",
  animal = {"zebra", "osprey", "rhinoceros", "cassowary", "donkey", "eel part"},
  group = {"group", "herd", "flock", "murder"},
  number = function() return 10 end,
  }

local grammar = Grammar(testGrammar)

text, data = grammar.generate("origin")

```

# API

## generate(symbol) -> text, data
Generates a piece of text from the given symbol name, and returns any pushed rules from the process in a table.

## addRule(symbol, value)
Pushes a rule onto the context of the grammar (destructive push).

## delRule(symbol)
Removes a rule from the context.

## pushContext(context)
Pushes a new context table onto the grammar stack (non destructive push).

## popContext()
Pops off a context stack and restores the previous context stack.

# Modifiers
Implements the same modifiers as Kate Compton's original tracery. Currently there is no way to add additional modifiers to a Grammar instance.

# Changes from the reference implementation
Grammars can contain functions as the value for a symbol. This function will be called with no arguments, and will have their return values inserted into the output text.

```lua
>>> grammar = Grammar({
		origin = "Your lucky number is: #number#",
		number = function()
			return math.random(42)
		end
	})

>>> text, _ = grammar.generate(origin)
>>> print(text)
Your lucky number is: 34

```

# Copyright
- lua-tracery is Copyright Aldous Rice-Leech 2019
- Tracery is Copyright Kate Compton 2015

See [LICENSE.md](/LICENSE.md) for more details