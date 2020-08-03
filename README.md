Tibor's Coding Standard
=========================

In software development, my mission is to write software that makes the world better.
This includes writing what I call 'humane code'. Code that helps colleagues (including my future self) in a friendly way to understand what's going on.
I've taken a lot of info from my career in medical psychology (neurology) and from several books that IMHO should be considered holy in software development, such as Clean Code by Robert C Martin and Domain Driven Design by Eric Evans
This project is onogoing, to challenge myself to write better code. 

##Content##
- General
- PHP and Symfony
- Javascript and AngularJS
- HTML / CSS
- Git

##General##
- Try to adhere to the 20/200 rule for methods and classes as much as possible.
- It is a very good habit to leave files you have worked in in a cleaner state than when you found them. If something annoys you and you can fix it in 5 min, do it.
- NEVER give variables names like $myArr, $someOb, $obj, $arr, etm. Always use declarative names that explain exactly what the variables contain. If I catch someone doing this, I will personally see to it that they don't get cookies for the rest of the week. I'm a friendly dude and I usually don't resort to this kind of violence. It's that bad. 
```
<?php
// Bad
$myArr = ['sword' => 100, 'magicSword' => 200 ];
foreach($myArr as $k => $v) {
    echo "$k has a combat power of $v"
}

// Good
$weaponsAndPower = ['sword' => 100, 'magicSword' => 200 ];
foreach($weaponsAndPower as $weapon => $power) {
    echo "$weapon has a combat power of $power"
}
```
- The only exception to the previous statement is iterator variables. But please call these $i, for 'iterator'. Not things like '$coco', '$fecalMatter' or '$yourMom'. Fun fact: I have actually seen these in the past in professionally written code. 
- Prevent nested loops and nested conditionals as much as possible. The more info you have to store in your brain's RAM, the slower you write code and the more insane you get. Nested loops eat brain RAM.
- 'Paragraph' your code. Insert blank lines between blocks of code that belong together. This makes your code easier to read and this helps your brain group info more efficiently and hence makes your code more awesome. This is especially true for Java and JavaScript. If this is not done, your code transforms into an uninviting wall of text.
```
<?php
// Bad
public function createDragonFromParameters(string $name, string $type, array $stats, Attack $attack, bool $hasWings, $lairStats = null): Dragon
{
    $dragon = new Dragon();
    $dragon->setName($name);
    $dragon->setType($type);
    $dragon->setStats($stats);
    $dragon->setAttack($attack);
    $dragon->setHasWings($hasWings);
    if (null !== $lairStats) {
            $lair = new Lair();
            $lair->setGoldAmt($lairStats['goldAmt']);
            if (array_key_exists('treasures', $lairStats)) {
                $treasures = [];
                foreach ($lairStats['treasures'] as $treasure) {
                    $treasures[] = $this->getTreasureService()->createTreasure($treasure)
                }
                $lair->setTreasures($treasures);
            } else {
                $lair->setTreasures(null);
            }
            $dragon->setLair($lair);
        }
    return $dragon;
}

// Good
public function createDragonFromParameters(string $name, string $type, array $stats, Attack $attack, bool $hasWings, $lairStats = null): Dragon
{
    $dragon = new Dragon();
    $dragon->setName($name);
    $dragon->setType($type);
    $dragon->setStats($stats);
    $dragon->setAttack($attack);
    $dragon->setHasWings($hasWings);

    if (null !== $lairStats) {
            $lair = new Lair();
            $lair->setGoldAmt($lairStats['goldAmt']);

            if (array_key_exists('treasures', $lairStats)) {
                $treasures = [];
                foreach ($lairStats['treasures'] as $treasure) {
                    $treasures[] = $this->getTreasureService()->createTreasure($treasure)
                }

                $lair->setTreasures($treasures);
            } else {
                $lair->setTreasures(null);
            }
            
            $dragon->setLair($lair);
        }

    return $dragon;
}
```
- When inserting todo tags in the code, clearly describe what should be done. 
- Check the annotations of a todo. If it's old, ask the writer if it's still relevant and proceed accordingly.
```
<?php
// Bad

/**
 * @todo: Needs fixing
 */
public function killMyMotherInlaw() 
{
    // ...
}

// Good
/**
 * @todo: This method is too long and does three things. It needs to be spliced out. 
 */
public function killMyMotherInlaw() 
{
    // ...
}
```
- todo tags go in the docblock. This prevents floating todo tags.
```
<?php
// Bad

// @todo: Airhockey is now also an olympic sport. This method cannot win airhockey yet.
/**
 * ...
 */
public function winTheOlympics() 
{
    // ...
}

// Good
/**
 * @todo: Airhockey is now also an olympic sport. This method cannot win airhockey yet.
 * ...
 */
public function winTheOlympics() 
{
    // ...
}
```
- Always let your editor autoformat your code when you're done working in a file (Ctrl + Alt + L in PhpStorm).
- Trust me on this, always respect character encodings. If you don't treat them with the respect they deserve, shit will hit the fan and the whole team will debug and rewrite code for 48h+ straight. Especially in multilingual applications. This is a must-read about character encodings for EVERY self-respecting dev: https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/

##PHP##

The PSR and Framework-related coding standards should be followed.

- NEVER EVER EVER use globals, superglobals, the global keyword, variable variables or variable method names. These are a general bad programming practise in any language. Here are important problems they cause: They will likely cause many unforseen errors, they are a pain to debug, can form security risks, they obfuscate code intent, confuse data with code, your editor helper methods do not work and I'm convinced that, if humanity overuses these, the devil himself will be summoned. If I see anyone use them, it will make me sad.
- Read the above again. You want to be a good programmer, don't you?
- Read the first point again. If I catch myself or someone else using any of those, you better pray that you brought your +2 platemail to work that day.
- Actually take the time to READ the PSR. Don't be lazy and let your IDE do everything for you. You want to learn, don't you? See https://www.php-fig.org/psr/
- Symfony coding standards: https://symfony.com/doc/current/contributing/code/standards.html
- Use PHP native functions if they are available for the problem you are working on. They are optimized.
- Strong type all the things. If you can't strong-type in many cases, it's a good sign that your code can be optimized. PHP has received a lot of hate for its typing in the past, IMO that was all legit. Strong types help your brain create a framework for what's going on, indicate intent and ensure no weird weak-typing bugs pop up.

###Operators###
- The ! operator  may only be used in comparisons for native php functions that explicitly return true or false. In all other cases (e.g. our own comparisons or 3rd party comparisons), === should always be used. IMO this is true for any weakly typed language. It prevents long nights debugging.
```
<?php
// Good
        if (!in_array('Mage', $playableClasses)) {
            //
        }

// Also good
        if (-1 === ourOwnCustomSearchMethodThatReturnsMinus1IfNotFoundPositionOtherwise('Mage', $playableClasses)) {
            //
        }

// Bad
        if (!ourOwnCustomSearchMethodThatReturnsMinus1IfNotFoundPositionOtherwise('Mage', $playableClasses)) {
            //
        }
```

- Don't concat strings and variables with the '.' operator if you can do it with "". That's what "" is invented for. It makes for an easier read in your IDE's colour scheme and therefore uses less of your brain's RAM.
```
<?php
// Bad
$message = 'Moderate lamentation is the right of the dead, excessive grief the enemy to the living.'

$output->writeln("Caught an Exception while persisting All's Well that Ends Well: " . $message);

// Good
$message = 'Moderate lamentation is the right of the dead, excessive grief the enemy to the living.'

$output->writeln("Caught an Exception while persisting All's Well that Ends Well: $message");

```
- Ternary conditionals may only be used if the comparison is really simple and easily fits in one line. Contrary to a rather polar belief, these do not make you look smart and awesome. It makes your code unreadable to everyone, including your future self. They use a lot of brain power and therefore slow dev speed of the team.
```
// Bad
return result != null ? result.id != -1 ? result.id : -1 : -1;

// Also Bad
$canCarryCoconut = $airspeedVelocityOfAnUnladenSwallow > 100 && $africanSwallow === true ? true : false;

// Good
$canCarryCoconut = $airspeedVelocityOfAnUnladenSwallow > 100 ? true : false;
```

###Variables and types###
- Variable variables ($$) should never be used in Business code. It obfuscates code and makes it hard to navigate through code. If you still think you should use variable variables, tell me what the following code will output without running it. 
- Note that I said "Business code" above. This can be an ok feature in meta code, such as ORMs, but even then they should be used with care.
``` 
<?php
$ch="a";$a="b";$b="c";$c="d";$d="e";$e="f";$f="g";$g="h";$h="i";$i="j";$j="k";$k="l";$l="m";$m="n";$n="o";$o="p";$p="q";$q="r";$r="s";$s="t";$t="u";$u="v";$v="w";$w="x";$x="y";$y="z";$z=" ";
echo("${$$$$$$$$$$$$$$$ch}${$$$$$$$ch}${$$$$$$$$$$$$$$$ch}${$$$$$$$$$$$$$$$$$$$$$$$$$$ch}${$$$$$$$$ch}${$$$$$$$$$$$$$$$$$$ch}${$$$$$$$$$$$$$$$$$$$$$$$$$$ch}${$$$$$ch}${$$$$$$$$$$$$$$$$$$$$ch}${$$$$$$$$$$$$$ch}\n");
```
If you still think these are awesome, let me tell you, you ain't seen shit yet, son. This is a bad feature. Just don't use it.

- When dealing with plain strings, use single quotes.

###Functions###
- A function should only do one thing, and do it well.
- A function name should be a clear description of exactly what it does. 
- All class methods should be declared either public, private or protected. 
- All functions should have a descriptive PHP docblock above them if they have multiple thought steps. Contrary to popular belief, not writing comments does not make you look smart and awesome. I've got dozens of neurology papers on how important it is to create a framework for your brain to group logic. A well-written comment in a docblock does just that. If you don't believe me, poke me, I'll share them with you.

```
<?php
// Good
    /**
     * Does the dishes by looking for the dishes specified in $dishes and passing the result to a helper method wash().
     * @param array $criteria
     * @return null|User
     */
    public function doTheDishes(array $dishes): ?User
    {
        // ...
    }

// Bad
    /**
     * Do the dishes
     * @param array $criteria
     * @return null|User
     */
    public function doTheDishes(array $dishes): ?User
    {
        // ...
    }
```

###Classes###
- 1 file, 1 class. No exceptions.
- A class should always have a docblock containing a description what it does. This will help your colleague's brain to group info. 
```
<?php
// Bad

class PokemonCatchingService {
// ...
}

// Also bad
/**
 * PokemonCatchingService
 */
class PokemonCatchingService {
// ...
}

// Also bad
/**
 * Catch pokemon
 */
class PokemonCatchingService {
// ...
}

// Good
/**
 * This Service is responsible for performing Catch operations on Pokemon. 
 * It contains methods for fetching Pokemon from the database, checking Pokemon integrity and catching Pokemon.
 */
class PokemonCatchingService {
// ...
}

```

##Javascript##
There is no single coding standard for javascript. I believe the Airbnb JS coding standard includes a lot of common sense and has a neat syntax. 
- If you use plain javascript, Javascript: the Good Parts by Douglas Crockford is a MUST read. This will help you understand the black magic of the language and improve your code quality greatly.  Yes, it's an old book. Yes, it's still very, very relevant.
- AirBNB: https://github.com/airbnb/javascript


###General###
- Don't use var. Var is global. As explained earlier, global is bad.
- Always declare variables explicitly.
```
// Bad
let planet;

// Also bad
planet = null;

// Good
let planet = null;

// Also good
let planet = {};

```
- Always use strict mode by typing 'use strict' at the top of your file. This prevents many headaches, such as implicit variables, dynamic properties, etm.
- It is generally a bad idea to use the ! operator in comparisons in weakly typed languages.  === should always be preferred. This prevents obscure type bugs.
- when checking a variable for undefined, use typeof.
```
// Bad
if (spaceShip === undefined)

// Good
if (typeof spaceShip === 'undefined')
```
- When dealing with plain strings, use single quotes.
- ```throw new Error();``` may ***only*** be used in a ```try{} catch()``` block.

###Angular Style Guides###
The John Papa standard makes great sense and should be followed in all files. See https://github.com/johnpapa/angular-styleguide 

Other styleguides that are useful:
- Todd Motto: https://github.com/toddmotto/angularjs-styleguide 
- Minko Gechev: https://github.com/mgechev/angularjs-style-guide

##HTML / CSS##
- Use HTML5 tags wherever possible. Yes, I know you like to abuse ```<div>``` tags. I like it too. But HTML5 tags explain intent and besides make your SEO score higher. If you have many divs, look for better tags to use.
- The use of element ids should be avoided for styling purposes.
- Do not use element ids for styling. Use classes instead. It makes me sad that I still encounter this in 2020+
- !important must be avoided as much as possible. If you need to fight against your own CSS precedence, you need to refactor. There are, however some cases where your chosen CSS framework needs to be overridden for some kind of design feature. If you use an !important tag, it must always be accompanied by a comment on why it is necessary.

##Git##
- Commits should always contain a clear description of what you did or a link to the feature/bug you worked on. Please help your colleagues create a mental framework of what the commit contains.
```
//Good
Fixed a bug on all Dashboard pages where an error 500 would occur if the client did not have RoundTypes.

//Bad
Fixed bugs Dashboard pages.
```
- Keep your commits in line with whatever you are working on.
- Files should be moved with `git mv`, not by yolo dragging and dropping them. The history of the file will be gone otherwise. The exception for this is if you've properly configured Git in your IDE.
- There's always a way to -un-mess=up your branch. Don't feel ashamed. It happens to everyone, everywhere. Ask a colleague. 