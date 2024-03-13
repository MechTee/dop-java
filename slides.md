---
#theme: apple-basic
colorSchema: light
#background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: true
info: |
  ## Java DOP
  Data-oriented Programming in Java
drawings:
  persist: false
defaults:
  foo: true
download: true
transition: slide-left
title: Immer nur objektorientiert - schon mal datenorientiert ausprobiert?
mdc: true
monaco: true
monacoTypesSource: cdn
hideInToc: true
---

# Immer nur objektorientiert - schon mal datenorientiert ausprobiert?


<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
Willkommen zu meinem Talk.
-->

---
layout: full
title: Inspiration
---

<img src="/jfs_itsme.jpg"/>
<!--
* Weiterbildung und Horizonterweiterung durch Konferenzen & Talks
* Nicht nur JAVA (auch bspw. RUST)
* JFS 2023
* Ron Veen's Modern Java - This is not your father's Java anymore
-->

---
layout: full
hideInToc: true
---
<img src="/inspiration-1920-1080.png"/>


---
transition: slide-up
layout: full
---

<img src="/that_damned_slide-1920-1080.png" class="h-full"/>


---
transition: slide-up
layout: default
hideInToc: true
---

# Table of contents

<Toc maxDepth="1"></Toc>

---
transition: slide-up
level: 1
layout: two-cols
---
# OOP

* complex entites and systems

<br>

* modelling business entites and processes

<br>

* great at defending boundaries

<br>

* leads to modular reasoning

::right::

<img src="/oop_dall-e.png" class="w-full"/>

---
transition: slide-up
level: 1
layout: two-cols
---

<img src="https://blog.klipse.tech/uml/chapter00/do-principles-mind-map.png" class="w-full"/>

::right::

# DOP

* model data instead

<br>

* consume from outside world

<br>

* less/no internal boundaries

<br>

* validation at service boundary


---
title: Principles of DOP
layout: two-cols
transition: slide-up
---

# Sharvit Principles
|   |
| ------ |
| 1. Seperate data from logic |
| |
| 2. Data is stored in generic data structures |
| |
| 3. Data is immutable |
| |
| 4. Seperate Schema from Data |

::right::

# Goetz/Java Principles

|    |
| ------ |
| 1. Model the data, the whole data, and nothing but the data |
| |
| 2. Make illegal states unrepresentable |
| |
| 3. Data is immutable |
| |
| 4. Validate at the boundary |

<style>
table, td, th, tr {
  border: none;
}
tbody tr:first-of-type td{
  height: 5rem;
}
</style>

---
layout: default
---

# Project AMBER and JEPs

* Switch Expressions
* Records
* Sealed Classes
* Pattern Matching


![java_timeline](/java_timeline.png)

---
level: 2
---

<Titles>
  <div class="flex w-full flex-row justify-between items-center">
    <div class="bg-orange-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">12</div>
    <h1 class="flex align-center justify-center items-center">Switch expressions JEP 361</h1>
    <div class="bg-green-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">14</div>
  </div>
</Titles>


````md magic-move
```java
var mOTD = ""; // meme of the day
switch (day) {
  case WEDNESDAY:
    mOTD =
      "ITS WEDNESDAY MY DUDES";
    break;
  case FRIDAY:
    mOTD = "FRIDAY, FRIDAY!";
    break;
  default:
    mOTD = "None found."
}
```

```java
var mOTD = switch (day) {
  case WEDNESDAY ->
    "ITS WEDNESDAY MY DUDES";
  case FRIDAY ->
    "FRIDAY, FRIDAY!";
    //ref: rebecca black
  default -> "None found."
}
```
````

---
layout: default
level: 2
---
<Titles>
  <div class="flex w-full flex-row justify-between items-center">
    <div class="bg-orange-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">14</div>
    <h1 class="flex align-center justify-center items-center">Records JEP 395</h1>
    <div class="bg-green-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">16</div>
  </div>
</Titles>

* classes that act as transparent carriers of immutable data
* nominal tuples

<div class="mt-10 flex justify-center align-items-center">
  <img class="w-80 h-80" src="/record.png"/>
</div>

---
layout: full
level: 2
---

````md magic-move
```java
record PokeDexEntry(int dexNo, String name) {}
```
```java
class PokeDexEntry {
  private final int dexNo;
  private final String name;

  PokeDexEntry(int dexNo, String name) {
    this.dexNo = dexNo;
    this.name = name;
  }

  int dexNo() {return dexNo;}
  String name() {return name;}

  public boolean equals(Object o) {
    if (!(o instanceof PokeDexEntry)) return false;
    PokeDexEntry other = (PokeDexEntry) o;
    return other.dexNo = dexNo && other.name == name;
  }

  public int hashCode() {
    return Objects.hash(dexNo, name);
  }

  public String toString() {
    return String.format("DexEntry[dexNo=%d, name=%s]", dexNo, name);
  }
}
```
````

---
layout: default
level: 2
---

<Titles>
  <div class="flex w-full flex-row justify-between items-center">
    <div class="bg-orange-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">14</div>
    <h1 class="flex align-center justify-center items-center">Sealed Classes JEP 409</h1>
    <div class="bg-green-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">16</div>
  </div>
</Titles>



````md magic-move

```java
interface Status {}

// non-volatile statuses need to be healed
interface NonVolatileStatus extends Status {}

record Burn(float dmgPercentage, boolean appliedAfterEnemyFaint) implements NonVolatileStatus {}


// volatile statuses subside after a random amount of turns
interface VolatileStatus extends Status {
  int turns();
}

interface PreventionStatus extends VolatileStatus {}

record Confusion(float chance, int turns) implements PreventionStatus {}
...
```

```java
sealed interface Status permits NonVolatileStatus, VolatileStatus {}

// non-volatile statuses need to be healed
sealed interface NonVolatileStatus extends Status permits Burn {}

record Burn(float dmgPercentage, boolean appliedAfterEnemyFaint) implements NonVolatileStatus {}


// volatile statuses subside after a random amount of turns
sealed interface VolatileStatus extends Status permits PreventionStatus {
  int turns();
}

sealed interface PreventionStatus extends VolatileStatus permits Confusion {}

record Confusion(float chance, int turns) implements PreventionStatus {}
...
```

```java
sealed interface Status permits NonVolatileStatus, VolatileStatus {}

// non-volatile statuses need to be healed
sealed interface NonVolatileStatus extends Status {

 record Burn(float dmgPercentage, boolean appliedAfterEnemyFaint) implements NonVolatileStatus {}
}

// volatile statuses subside after a random amount of turns
sealed interface VolatileStatus extends Status permits PreventionStatus {
  int turns();
}

interface PreventionStatus extends VolatileStatus {

  record Confusion(float chance, int turns) implements PreventionStatus {}
}


```


```java
sealed interface Status permits NonVolatileStatus, VolatileStatus {}

// non-volatile statuses need to be healed
sealed interface NonVolatileStatus extends Status {

  record Burn(float dmgPercentage, boolean appliedAfterEnemyFaint) implements NonVolatileStatus {}
}

// volatile statuses subside after a random amount of turns
sealed interface VolatileStatus extends Status permits PreventionStatus {
  int turns();
}

non-sealed interface PreventionStatus extends VolatileStatus {}

record Confusion(float chance, int turns) implements PreventionStatus {}
...
```

````

<!--

main motivation: widely accessible but not widely extensible.

Constraints:
All permitted subclasses must belong to the same module as the sealed class.
Every permitted subclass must explicitly extend the sealed class.
Every permitted subclass must define a modifier: final, sealed, or non-sealed.

* with (final) records we have **algebraic data types!**
-->

---
layout: default

---

<Titles>
  <div class="flex w-full flex-row justify-between items-center">
    <div class="bg-orange-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">14</div>
    <h1 class="flex align-center justify-center items-center">Pattern matching for instanceof JEP394</h1>
    <div class="bg-green-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">16</div>
  </div>
</Titles>

````md magic-move

```java
if (obj instanceof Status) {
  // grr...
  Status s = (Status) obj;
  ...
}
```

```java
if (obj instanceof Status s) {
  // Let pattern matching do the work
  ...
}
```

````


<div class="w-full flex gap-8 mt-8">
  <div v-click class="w-1/2">
  
  ```java
  if (a instanceof Status s) {
    ...// s is in scope
  }
  // s not in scope here
  if (b instanceof Status s) {
    ...
  }
  ```

  </div>
  <div v-click class="w-1/2 flex flex-col">

  ```java
  if (obj instanceof Confusion c && c.turns() > 1) {
    // works
  }
  ```

  ```java
  if (obj instanceof Confusion c || c.turns() > 1) {
    // doesn't work :(
  }
  ```

  </div>
</div>

<style>
div.slidev-code-wrapper.slidev-code-magic-move.relative {
  margin-top: 2rem !important;
}
</style>

---
layout: default
level: 2
---

<Titles>
  <div class="flex w-full flex-row justify-between items-center">
    <div class="bg-orange-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">19</div>
    <h1 class="flex align-center justify-center items-center">Record Pattern Matching JEP 440</h1>
    <div class="bg-green-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">21</div>
  </div>
</Titles>

````md magic-move

```java
if (obj instanceof Confusion c) {
  int turns = c.turns();
  if (turns > 0) {
    float chance = c.chance();
    boolean isConfused = calcConfusionCheck(c.chance());
    if (isConfused) {
      // grr...
      println("It hurt itself in it's confusion.");
    }
  }
  ...
}
```

```java
if (obj instanceof Confusion(int turns, float chance)) {
  if (turns > 0) {
    boolean isConfused = calcConfusionCheck(chance);
    if (isConfused) {
      // grr...
      println("It hurt itself in it's confusion.");
    }
  }
  ...
}
```

````

---
layout: default
level: 2
clicks: 5
---

<Titles>
  <div class="flex w-full flex-row justify-between items-center">
    <div class="bg-orange-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">17</div>
    <h1 class="flex align-center justify-center items-center">Pattern Matching for switch JEP 441</h1>
    <div class="bg-green-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">21</div>
  </div>
</Titles>

````md magic-move

```java
if (status instanceof MajorStatus mStatus) {
  applyMajorStatus(mStatus);
} else if (status instanceof DamagingStatus dStatus) {
  applyDamagingStatus(dStatus)
} else if (status instanceof PreventionStatus pStatus) {
  applyPreventionStatus(pStatus)
} ...
// there are 12 volatile statuses
```

```java
switch (status) {
  case MajorStatus mStatus ->
    applyMajorStatus(mStatus);
  case DamagingStatus dStatus ->
    applyDamagingStatus(dStatus);
  case PreventionStatus pStatus ->
    applyPreventionStatus(pStatus);
  ...
}
```

````
<!-- TODO: maybe refactor this click behaviour -->
<div v-click></div>
<div class="flex w-full gap-4 mt-10">
<div v-click="[2,6]" class="w-1/2 flex flex-auto">

````md magic-move

```java
switch (status) {
// Case refinement no guards:
  case VolatileStatus vStatus ->
    if (vStatus.turns() > 0) {
      applyVolatileStatus(vStatus);
    } else {
      removeStatus(vStatus);
    }
}
```

```java
switch (status) {
// Case refinement with guards:
  case VolatileStatus vStatus
       when vStatus.turns() == 0 ->
    removeStatus(vStatus);
  case VolatileStatus vStatus ->
    applyVolatileStatus(vStatus);
}
```

````
</div>
<div v-click></div>
<div v-click="[4,6]" class="w-1/2 flex flex-auto">

````md magic-move

```java
if (s == null) { // extra null handling
  println("😭")
}
switch (s) {
  case "FOO", "BAR" -> println("😂");
  default -> println("😐");
}
```

```java
// switch handles null
switch (s) {
  case null -> println("😭");
  case "FOO", "BAR" -> println("😂");
  default -> println("😐");
}
```

````

</div>

</div>

<style>
div.w-1\/2.flex.flex-auto div {
  width: 100%
}
</style>

---
layout: default
level: 2
clicks: 4
transition: slide-up
---

<Titles>
  <div class="flex w-full flex-row justify-between items-center">
    <div class="bg-orange-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">21</div>
    <h1 class="flex align-center justify-center items-center">Unnamed Variables & Patterns JEP456</h1>
    <div class="bg-green-600 shadow h-10 w-10 text-center justify-center items-center flex mb-4">22</div>
  </div>
</Titles>

````md magic-move

```java
static int countStatuses(
    Iterable<Status> statuses) {
  int total = 0;
  for (Status status: statuses)
    total++;
  return total;
}
```

```java
static int countStatuses(
    Iterable<Status> statuses) {
  int total = 0;
  for (Status _ : statuses)
    total++
  return total;
}
```

````
<div v-click></div>
<div v-click="[2,6]" class="mt-10">

````md magic-move

```java
switch (box) {
  case Box(RedBall red) ->
    processBox(box);
  case Box(BlueBall blue) ->
    processBox(box);
  case Box(GreenBall green) ->
    stopProcessing();
  case Box(var nullBall) ->
    pickAnotherBox();
}
```

```java
// refinement with unnamed variables
switch (box) {
  case Box(RedBall _),
       Box(BlueBall _) ->
    processBox(box);
  case Box(GreenBall _) ->
    stopProcessing();
  case Box(var _) ->
    pickAnotherBox();
}
```

```java
// further refinement with unnamed pattern:
switch (box) {
    case Box(RedBall _), Box(BlueBall _) -> processBox(box);
    case Box(GreenBall _)                -> stopProcessing();
    case Box(_)                          -> pickAnotherBox();
}
```

````

</div>

---
layout: two-cols
level: 1
transition: slide-up
---
# DEMO: 
* Pokemon need healing after fights
* New Pokemon Generations introduced new Regions and PokeDex
* Different regional Pokemons require different Healing (assumption)

::right::

<img src="/nurse_joy.png" class="w-full" />

---
layout: default
---

# How do others do it? - Kotlin

<div class="flex flex-row w-full">
<div class="flex w-1/2 pr-10">

* record-"like" data classes
* sealed classes and interfaces (Kotlin 1.5)
* `when` pattern matching is limited (KT-4608, KT-13626)

</div>

<div class="flex w-1/2">

<div class="flex w-1/2">
</div>
<div class="flex w-1/2">
<img src="/Kotlin_Icon.png" class="h-32"/>

</div>
</div>
</div>
<div class="mt-8">

```kotlin
sealed interface Pokemon {
  data class KantoPokemon(val nickName: String) : Pokemon
  data class JohtoPokemon(val nickName: String) : Pokemon
  data class SinnohPokemon(val nickName: String) : Pokemon
}

// healingService
fun healPokemon(pokemon: Pokemon) = when (pokemon) {
  is KantoPokemon -> healKantoPokemon(pokemon)
  is JohtoPokemon -> healJohtoPokemon(pokemon)
  is SinnohPokemon
}

```

</div>


---
layout: default
transition: slide-up
---

# How do others do it? - Rust

<div class="flex flex-row gap-4">
<div class="flex w-1/2">

* data oriented sees shared mutable data as a very bad thing
* rust: manage access to that shared data and who can mutate

</div>

<div class="flex w-1/2">

<div class="flex w-1/2">
</div>
<div class="flex w-1/2">
<img src="/rustacean-flat-happy.png" class="h-32"/>

</div>
</div>
</div>

<div class="flex flex-1 flex-row w-full gap-4 mt-8">
<div v-click class="w-1/2">

```rust
pub enum Option<T> { // Sealed class with records :O
  /// No value.
  None,
  /// Some value of type `T`.
  Some(T),
}
```

</div>

<div v-click class="w-1/2">

```rust
pub enum Result<T, E> {
  /// Contains the success value
  Ok(T),
  /// Contains the error value
  Err(E),
}
```

</div>

</div>

<div class="flex flex-row flex-1 w-full gap-4 mt-4">

<div class="w-1/2" v-click>

```rust
let option = Some("Value");
match option { //extensive pattern matching
  Some(val) => println!("We have a {}", val),
  None => println!("We have no value")
};
```

</div>

<div class="w-1/2" v-click>

```rust
let result = Err("OW it did not worky")

if let Err(error_msg) = result {
  println!("Error occured: {}", error_msg);
} // else or continue flow
```

</div>
</div>


---
layout: default
---

<!-- todo: add some image! -->

# Conclusion
* DOP is a nice addition to OOP
* useful especially for small units like microservices
* mainly handling outside data
* Try it some time :)

---
layout: quote
transition: slide-up
---

<div class="flex justify-center">
Don't be a Functional programmer, don't be an Object Oriented programmer, be a better programmer.
</div>
<!--TODO: maybe this could be better-->
<div class="flex justify-end mt-8">
Brian Goetz
</div>

---
transition: slide-up
hideInToc: true
layout: full
title: Q & A
---

<img src="/questions.jpg" class="h-full" />

---
hideInToc: true
layout: end
---

# THANK YOU & contact me here:

- E-Mail: mechti.oezdogan@exxeta.com
- Github: https://github.com/MechTee/
- LinkedIn: https://www.linkedin.com/in/mechtee/

## Slides here:
<!-- TODO: update this!-->
<div class="flex justify-center mt-8">
  <img class="h-40" src="/qr-code-drive.png"/>
</div>
