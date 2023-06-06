# Towers of Hanoi

Let's implement a Towers of Hanoi game in JavaScript!

## Learning goals

By the end of this practice, you should be able to

- Use pseudocode to structure your reasoning
- Use JavaScript classes and objects in Node
  - Use Object-Oriented Programming (OOP) principles in JavaScript
- Test methods as you write them
- Implement I/O using readline

## Pseudocode

Before you start, write out `Game.prototype.run` in pseudocode (using comments).
For example, if you were to write the pseudocode for `Chess.prototype.run`, it
would look something like this:

```javascript
class Chess {
  run() {
    // until a player is in checkmate
    // get move from current player
    // make move on board
    // switch current player
  }
}
```

Save the pseudocode to a separate file. You'll come back to it later.

## `Game` class

Now write a `Game` class and run it using Node:

- In the `constructor`, set a property for the stacks (perhaps `this.towers`).
  It should be an array containing 3 subarrays (stacks). The last two stacks
  should be empty; the first should be `[3, 2, 1]`, where the last element, `1`,
  represents the smallest disc at the top of the stack.

- Write a `print` method to print the stacks. A basic approach would be to call
  `JSON.stringify` on `this.towers` to turn it into a string, brackets and all
  (like Ruby's `p` method). You can format the stacks more if you'd like, but
  spend no more than five minutes on formatting. Test to make sure this method
  works.

- Write a `promptMove(callback)` method that (1) prints the stacks and (2)
  generates a random (0, 1, or 2) `startTowerIdx` and `endTowerIdx`--i.e., where
  the disc is coming **from** and going **to**, respectively--and passes them to
  the `callback` argument. You'll adjust this later to take user input via the
  terminal.
  - Test `promptMove` by passing in a callback that simply `console.log`s the
    `startTowerIdx` and `endTowerIdx` (and does not run any other code).

- Write an `isValidMove(startTowerIdx, endTowerIdx)` method that checks whether
  or not you can move the top of `startTowerIdx` onto the top of `endTowerIdx`.
  (Larger discs cannot go on top of smaller discs.)
  - Test it with two cases: 1) a valid case where the top disc at
    `startTowerIdx` is smaller than the top disc at `endTowerIdx` and 2) an
    invalid case where the top disc is larger. You may need to tweak your
    starting `this.towers` to set up an invalid case.

See the theme here? Test each method, one at a time, before you move on.

- Write a `move(startTowerIdx, endTowerIdx)` method that performs the move only
  if it is valid. Return true/false to indicate whether or not the move was
  performed. Test for both valid and invalid moves.

- Write an `isWon` method that checks the stacks to see if all discs were moved
  to a new stack. Test for both winning and non-winning conditions.

- Write a `run(completionCallback)` method.
  - `promptMove` from the simulated user (you'll add real user input next).
  - In the `promptMove` callback, try to perform the move. If it fails, print an
    error message. Test that `run` is working so far.
  - If the game is not yet won, call `run` again.
  - Otherwise, log that the user has won, then call the `completionCallback`.
  - Test this out: should you call `isWon` in the top level of `run` or in the
    callback to `promptMove`?

Compare the `run` method you wrote to your original pseudocode. Does it look
like what you expected?

## Playing the game

To play your game, you need to implement a layer of I/O to connect your game
logic to user input. Create a __playScript.js__ file.

- Import your game by requiring __./game.js__ as `Game`.

- Instantiate an I/O reader using Node's `readline` library:

  ```js
  const readline = require('readline');
  
  const reader = readline.createInterface({
    input: process.stdin,
    output: process.stdout
  });
  ```

  The `readline` library is built into Node and covers input and output
  functionality. The `reader` instantiated above will look for input from the
  terminal (`stdin`) and also output to the terminal (`stdout`).
  
  You'll use the `readline` library more later. For now, feel free to check out
  its [documentation][readline].

- In your `Game` class, refactor both `Game.prototype.run` and
  `Game.prototype.promptMove` to take in `reader` as their first arguments (and
  callbacks as their second arguments).
  - Within `Game.prototype.run`, be sure to pass `reader` to both `promptMove`
    and `run`.
  - Within `promptMove`, replace the randomly generated `startTowerIdx` and
    `endTowerIdx` with values provided by the user. Use the `reader`'s
    [`question`] method. (You might need to write some nested callbacks!)

- Back in __playScript.js__, instantiate a new `Game` and call `run` on it,
  passing the `reader` and a completion callback that asks the user, via
  `reader.question`, if they want to play again.
  - If they do want to play again, instantiate a new `Game` and call `run` on
    it.
  - If they don't want to play again, [`close`] the reader.

Go ahead and run the __playScript.js__ file to play a game!

[readline]: https://nodejs.org/dist/latest-v16.x/docs/api/readline.html
[`question`]: https://nodejs.org/api/readline.html#rlquestionquery-options-callback
[`close`]: https://nodejs.org/api/readline.html#rlclose
