# Career growth for software engineers: becoming a leader

> Date: 26 April 2022
##### [Published here](https://anywhere.epam.com)
##### [Link to published document](https://anywhere.epam.com/en/blog/javascript-functional-programming-vs-oop)
&nbsp;
___

### Introduction

Functional programming is a trending approach when it comes to user interface development. Why? Is it because object-oriented programming (OOP) is too complicated, overpriced, and old-school? Let's try to sort out why there's a 'functional programming vs OOP' narrative in the programming community.

In fact, developers often prefer to use a function-oriented programming style rather than OOP. Everyone knows that OOP features encapsulation, inheritance, polymorphism, and SOLID. Of course, with JavaScript, things work a little bit differently. OOP has a prototype inheritance which, for some reason, is not widely discussed or used, and Object has null in its prototype.

Function-oriented programming is an effective technique based on the functional factors required for developing various programs. Unfortunately, developers often master just a couple of functional programming techniques. This is understandable since it is easier to learn three or four function-oriented programming techniques than to deep dive into the depths of OOP.

What's the primary difference between object-oriented programming and functional programming?

### The key difference between object-oriented programming and functional programming

To identify what distinguishes functional programming vs OOP, you need to understand the fundamental basis, the smallest unit that each uses. Different disciplines have different fundamental bases. In math, it is numbers; in literature, words; in physics, quarks; in chemistry, elements. The rules, structures, and other complex notions relevant to each are created based on this smallest unit.

In OOP, the fundamental basis is classes, instances of which can be stored in variables. In functional programming, however, there are no variables; there are functions and functions only.

#### Principles

During candidate interviews, I’ve observed that most candidates from Junior to Lead roles can enlist closures, pure functions, and immutability. I rarely hear about composition, idempotency, finite state machines, and category theory. This is disturbing. OOP is a familiar topic: everyone knows about abstraction, encapsulation, inheritance, polymorphism, and the concepts behind DRY and KISS. Most of the candidates, however, prefer to bypass design patterns and SOLID.

Functional programming becomes salvation from an OOP interview because not everyone has the knowledge or background to discuss it in depth.

To see the difference between functional programming vs OOP, let's examine a common composition from the position of JavaScript, using the example of several calculator operations based on user input. The main task is to eliminate the need to store anything. We have an entry point and an exit point and we don't need anything more:

```html
<script>
     // Two pure functions that have no side effects i.e. they don't mutate arguments
     // also act according to two principles
     // 1. Least surprise - the function does exactly what is expected from it
     // 2. Idempotency - the function always returns the same result for the same arguments
     // The cherry on the top, as developers don't like writing unit tests due to
     // ambiguity of arguments and strange behavior of test objects,
     // in functional programming this makes everything much easier

     // Self-expansion of passed arguments
     const summator = (...values) => { 
         return values.map((value) => value + value) 
     } 

     // Multiply the passed arguments
     const multiplicator = (...values) => { 
         return values.reduce((accumulator, value) => value * accumulator) 
     } 

     // Multiply by 2 passed arguments
     const x2multiplicator = (...values) => { 
         return values.map((value) => { 
             return multiplicator(value, 2) 
         })
     }

     // It would be possible to get rid of return for beauty (I did it below), but here I left it
     // to understand that this is an application of a few more principles
     // 1. Closures, for a function lower than compose, the arguments to functions do not exist
     // they are already in their scope, in OOP this can be called partial encapsulation
     const compose = (...functions) => {
         return (...values) => {
             // 2. Map-Reduce - of course, we can talk about the parallel processing model of big data
             // but here we are talking about taking datasets and transforming them, and after
             // work with tuples (key-values) to decompose it into smaller sets
             // this allows you not to store the state, but to transfer it, thereby we achieve
             // the most effective idempotency, composition, and immutability itself, thanks to, among other things, the spread-operator
             return functions.reduce((accumulator, functor) => { 
                 return functor(...accumulator)
             }, values)
         } 
     }

     // Again closures to get rid of unnecessary arity and scope of internal functions
     // Also this is an attempt to replicate modules from Elixir, Ruby, Erlang
     const getHandlers = () => { 
         const get = () => { 
             return [
                 parseInt(document.getElementById('left').value, 10), 
                 parseInt(document.getElementById('right').value, 10),
             ]
         }

         const set = (value) => {
             // It would be appropriate here to call the API and store the value in the database, for example
             alert (value)
         } 

         return { 
             get, set 
         } 
     }
 
     const onInit = () => {
         // Our linker returns a function that also accepts a value for
         // apply relative to passed operations
         // Unfortunately, JavaScript does not have a complete implementation of monads
         // we cannot set arguments in convenient order for us by curling functions
         // however, we can do this with sequential arguments, it turns out
         // a kind of Builder pattern
         // like below, we can set arguments along with context
         // this is also a kind of closure, but the purpose is different - to define a set of arguments
         // in convenient order for us, and return or accept another function as a result or argument
         // these are called higher-order functions
         const composeFunctors = compose.call(this, x2multiplicator, summator, x2multiplicator) 
         const { get, set } = getHandlers()

         // Get the data -> multiply by 2 -> add it -> multiply by 2 -> output the data
         // [2,3] -> [4,6] -> [8,12] -> [16, 24]
         // Thanks to the spread operator in all our functions, we can pass any number of argumenеs
         // For example - set (composeFunctors (... [1,2,3,4,5,6])) becomes [8,16,24,32,40,48]
         set (composeFunctors (... get ()))
     }
</script>

<input id = "left" placeholder = "Enter the first value" value = "3" />
<input id = "right" placeholder = "Enter the second value" value = "2" />

<button id = "button" onclick = "onInit ()"> Calculate! </button>
```

Below is the same code without unnecessary returns and comments; a similar idea is implemented in Redux, with state machines and that's all:

```javascript
const summator = (...values) => values.map((value) => value + value) 
const multiplicator = (...values) => values.reduce((accumulator, value) => value * accumulator) 
const x2multiplicator = (...values) => values.map((value) => multiplicator(value, 2)) 

const compose = (...functions) => (...values) => functions.reduce((accumulator, functor) => functor(...accumulator), values) 

const getHandlers = () => ({ 
    get: () => [
          parseInt(document.getElementById('left').value, 10),
          parseInt(document.getElementById('right').value, 10),
    ], 
    set: (value) => alert(value) 
}) 

const onInit = () => { 
        const composeFunctors = compose.call(this, x2multiplicator, summator, x2multiplicator)
        const { get, set } = getHandlers() 
        set(composeFunctors(...get()))
}
```

### Other differences between object-oriented programming vs functional programming

In addition to the fundamental distinction between the two, other differences between object-oriented programming and functional programming include:

- **Data usage**: Functional programming uses immutable data while OOP uses mutable data.
- **Programming model**: Functional programming follows a declarative model while OOP follows an imperative model.
- **Support**: Advocates for functional programming adhere to the ideology of pipelines, which conveys the ideas of state machines accurately enough. OOP (true OOP) has tight relations between class elements, which doesn't allow the code to be fully modular.
- **Execution**: In functional programming vs. OOP, functions are atomic, and are isolated from the context as much as possible. As a result, you can reuse functions anywhere since they are not linked to modules.
- **Element**: The basic elements of functional programming are functions while the basic elements of OOP are objects and methods.
- **Use**: Due to its lack of ambiguity, the level of fault tolerance in functional programming is extremely high, so you can achieve reliability approaching 99.9999%.

### Final thoughts

Functional programming can be effectively applied in JavaScript, but it should be applied properly, with an understanding of the concept of state machines, for example. Employed this way, programming using Redux, GraphQL, and solutions like Redis / MongoDB might be quite interesting. You can try to completely eliminate the imperative style, leave data transfer and processing to the user, and focus on the business logic instead.

The key difference between object-oriented programming vs functional programming is that object-oriented programming attempts to operate using abstractions, rather than code, so that a developer can use known patterns and master the highest level. Functional programming, on the other hand, pushes us towards what Robert Martin said when refactoring the Date class, that the minimum arity of functions is required (basically, that zero is the optimal number of arguments for a function).

Of course, you can chase math and try to write good REST in Haskell, but it's important to understand that functional programming in the right hands provides predictable fault tolerance and testability under high loads. On the other hand, there are those who advocate for OOP in the OOP vs functional programming debate. Ideally, professionals in both camps should try their hand at JavaScript to see how you can make the paradigms work together in a twisted way.
