<!-- Get AI to Play a Move -->
<section
  id="the-ai-plays-a-move"
  aria-labelledby="the-ai-plays-a-move"
  data-item="The AI Plays a Move"
>
  <h2><a href="#the-ai-plays-a-move">The AI Plays  a Move</a></h2>
  
<details class="challenge" open>
<summary>Challenge 2: Can you get the AI to play a move, and claim instructions?</summary>

Before, only the human player was playing, so you could use the fixed phrase `You hit the nail `, and then add `gently` or `firmly` or `hard` after it. Now you'll need to get the the AI to say `I hit the nail ...`.

```bash-w
<i>-============| The nail is 13 units long.

==========|    You hit the nail gently.</i>

<b>=========|     I hit the nail gently.</b>
```
</details>

## Coercion

You've already seen that JavaScript allows you to use truthy values like `12` to act as if they were `true`. You've heard that `0` will behave like `false`. JavaScript can make variables of one type behave like variables of a different type.

JavaScript uses a process called _coercion_ when you try to combine values of two different types (like Boolean `true` or `false`) and numbers. Using coercion, JavaScript will convert one of the types into the other.

<details class="sandbox" open>
<summary>Turn `false` into `0`</summary>
Here's a trick for you to try in the Node IDE, to turn `false` into `0` and `true` into 1:

1. In a new Terminal, type `node`, and then the following expressions:

```bash-w
<b>node</b>
Welcome to Node.js v23.1.0.
Type ".help" for more information.
> <b>false + 0</b>
0
> <b>true + 0</b>
1
```

And just for fun:

```bash-w
> true + false
1
```

## Coercing numbers and strings

Sometimes coercion has unexpected consequences. Try this in the Node IDE:

```bash-w
> 4 + "2"
'42'
> 4 - "2"
2
```
The string “42” is created because `+` can be used as a [string concatenation operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_operators#string_operators), and so that's how JavaScript decides to use it. The number 4 is coerced to a string. The [`-` subtraction operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Subtraction) is only used with numbers, so the string "2" is coerced to a number before the operation.

</details>

## Using an array to get the current player's pronoun

You could create an array with the different pronouns that your game needs: "I" for the AI, "You" for the human player that the AI is talking to.

```javascript
const players = [ "I", "You" ]
```

The value of `players[0]` will be "I", while `player[1]` will be "You". The value of the variable player will be either `true` or  `false`. From what you tested above, can you see how to get the value ”I” from players when player is `false`, and how to get the value ”You” when player is `true`?

<details class="solution">
<summary>Solution to the pronoun problem</summary>

You can test this your solution in the Node IDE:

```bash-w
<b>node</b>
Welcome to Node.js v23.1.0.
Type ".help" for more information.
> <b>players = [ "I", "You" ]</b>
[ 'I', 'You' ]
> <b>player = true</b>
true
> <b>players[player + 0] + " hit the nail"</b>
'You hit the nail'
> <b>player = false</b>
false
> <b>players[player + 0] + " hit the nail"</b>
'I hit the nail'
```

</details>

<details class="hint">
<summary>Hints for solving the main challenge</summary>
1. You can create an array called `players` with the value [ "I", "You" ]
2. You can change your current `hit` string to ` hit the nail `
3. You can use...
   ```javascript
   prompt = players[player + 0] + hit + strength[index] + "."
   ```
   ... to generate a prompt that will work for either player.
4. Should you move this code from where it is to a better place?
5. Currently, `index` is declared as a `const` in the `if (player) { ... }` block, so it is not available outside that block. Can you declare it with `let` outside the `if` statement altogether?
6. When the AI chooses a value for `force`, you will need to use an adjusted value of `index` to get the correct word from the `strengths` array.  
7. For now, you can make the AI use a `force` of `1` every time.

</details>

<details class="solution">
<summary>Solution to the main challenge</summary>

You can achieve the required result by making **changes** in six different lines of code:

```javascript-
<i>const {
  keyInYN,
  keyInSelect
} = require('readline-sync')

const rules = `Let's knock a nail into this computer!

* Each player takes a turn to hit the nail once.
* A player can hit the nail in one of three ways:
  gently, firmly, hard.
* Depending on the force used, the nail will be
  driven more or less deeply into the Terminal.
* The player who knocks the nail all the way in
  is the winner.

Are you ready?
`
const whoStarts = `If you want to start, type Y.
If you want me to start press any other key. `
const nailIs    = "The nail is "
const long      = " units long."
const clear     = "\x1B[1A\x1B[K"
const strength  = [
  'gently',
  'firmly',
  'hard'
]
const question = 'How hard do you plan to hit?'
const players  = [ "I", "You" ]
const hit      = " hit the nail "
const win      = `
You win!
`
const endGame  = `Thanks for playing!
`

const initial = 12 + Math.floor(Math.random() * 4)
let length    = initial
let toDelete  = 14
let prompt    = nailIs + length + long
let started   = false
</i><b>let index</b><i>
let force
let nail

console.log(rules)
let player = keyInYN(whoStarts)

while (length > 0) {
  if (!started) {
    nail = "-" + "=".repeat(length - 1) + "|"
  } else {
    nail = "=".repeat(length) + "|"
  }

  console.log(clear.repeat(toDelete))
  console.log(nail, prompt)

  if (player) { // it's the human player's turn
    </i><b>index = keyInSelect(strength, question)</b><i>
    if (index < 0) {
      console.log(clear.repeat(toDelete))
      console.log(endGame)
      process.exit()
    }
    force = Math.min(index + 1, length)
    toDelete = 7
  } else { // it's the AI's turn to play
    toDelete = 0
    </i><b>force = 1
    index = 0</b><i>
  }

  </i><b>prompt = " ".repeat(initial - length + force)
         + players[player + 0] + hit + strength[index] + "."</b><i>

  length = length - force
  started = true
  player = !player
}

console.log(clear.repeat(toDelete))
console.log("|", prompt)
console.log(win)</i>
```
</details>
</section>