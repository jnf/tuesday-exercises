# Understanding `this`
##### Well, maybe not understanding, but being less angry at `this`. That's a good start.

## `this` in a grammatical context
##### `this` is a pronoun.

Read this sentence:
`Jeremy is broke because he spends all his money on records.`

`he` is a pronoun. `Jeremy` is the pronoun's _antecedent_ (the word to which the pronoun refers). We use pronouns to make language less formal and less redundant. `Jeremy is broke because Jeremy spends all his money on records.` is totes correct grammar, but it feels weird to say.

Ok, now read this code:
```javascript
  var budget = {
    recordStoreDayMonth: 'April',
    rsdFactor: 10,
    records: 25,
    forRecords: function(month) {
      if(this.recordStoreDayMonth === month) {
        return this.records * this.rsdFactor
      } else {
        return this.records
      }
    }
  }

  // I'm not broke in March
  budget.forRecords('March'); // 25

  // I'm ridiculously broke in April
  budget.forRecords('April'); // 250
```

`this` is to the object `budget` as `he` is to `Jeremy`. It's a pronoun, with an antecedent, used to add clarity to the surrounding statement.

In the preceding code, we could absolutely say `return budget.records * budget.rsdFactor`, but that introduces ambiguity because there could be other variables also named `budget` in scope, depending on how the `forRecords()` method is called.

## `this` does not exist until the function is _invoked_.

I shamelessly stole (and lightly edited) this sentence from [Javascript Is Sexy](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/) blog. It's a good sentence (emphasis mine):

> Even though it _appears_ `this` refers to the object where it is defined, it is not until an object invokes the function that `this` is actually assigned a value. And the value it is assigned is based __exclusively__ on the object that invokes the this Function.

Here's an example showing how invocation can affect context:

```javascript
  var responsibleBudget = {
    recordStoreDayMonth: 'April',
    rsdFactor: 5,
    records: 20
  }

  // apply()'s first argument is the contextual object.
  // the second argument is an array whose contents are
  // fed to the called method (forRecords in this example)
  var marchBudget = budget.forRecords.apply(responsibleBudget, ['March']);
  var aprilBudget = budget.forRecords.apply(responsibleBudget, ['April']);
```
##### So what's going on here?
- What code is executed when these lines are executed?
- What's the value of `this` in the forRecords() method on execution?
- What are the final values of `marchBudget` and `aprilBudget`?
