### Garbage collection

> #### Slide 1
> Garbage collection is when the JavaScript's engine that runs your code freeze of memory for you automatically. In many lower level programming languages you have more control of memory. It means that you might be able to say "reserve this part of memory for me!". And also you might have to say "free up this part of memory for me!". And JavaScript doesn't do that. 
> Memory management in JavaScript is performed automatically and invisibly to us. We create primitives, objects, functions… All that takes memory.
> What happens when something is not needed anymore? How does the JavaScript engine discover it and clean it up?

> #### Slide 2

> The main concept of memory management in JavaScript is ***reachability***.
> In simple words, “reachable” values are those that are accessible or usable somehow. They are guaranteed to be stored in memory.
> There’s a base set of inherently reachable values, that cannot be deleted for obvious reasons.

> #### Slide 3

> For instance:
> * Local variables and parameters of the current function.
> * Variables and parameters for other functions on the current chain of nested calls.
> * Global variables.
> * (there are some other, internal ones as well)

> These values are called roots.
> Any other value is considered reachable if it’s reachable from a root by a reference or by a chain of references.
> For instance, if there’s an object in a local variable, and that object has a property referencing another object, that object is considered reachable. And those that it references are also reachable. 
> There’s a background process in the JavaScript engine that is called garbage collector. It monitors all objects and removes those that have become unreachable.

> #### Slide 4

> Here’s the simplest example:

> #### Slide 5

> Here the arrow depicts an object reference. The global variable "user" references the object {name: "John"}. The "name" property of John stores a primitive, so it’s painted inside the object.

> #### Slide 6

> If the value of user is overwritten, the reference is lost:

> #### Slide 7

> Now John becomes unreachable. There’s no way to access it, no references to it. Garbage collector will junk the data and free the memory.

> #### Slide 8 && Slide 9

> Now let’s imagine we copied the reference from user to admin:

> #### Slide 10

> Now if we do the same:
> …Then the object is still reachable because of admin global variable, so it’s in memory. If we overwrite admin too, then it can be removed.

> #### Slide 11

> Now a more complex example. The family:
> Function marry “marries” two objects by giving them references to each other and returns a new object that contains them both.

> #### Slide 12

> The resulting memory structure:
> As of now, all objects are reachable.

> #### Slide 13

> Now let’s remove two references.

> #### Slide 14

> It’s not enough to delete only one of these two references, because all objects would still be reachable.

> #### Slide 15

> But if we delete both, then we can see that John has no incoming reference any more.
> Outgoing references do not matter. Only incoming ones can make an object reachable. So, John is now unreachable and will be removed from the memory with all its data that also became unaccessible.

> #### Slide 16

> After garbage collection:

> #### Slide 17

> It is possible that the whole island of interlinked objects becomes unreachable and is removed from the memory.
> The source object is the same as before. Then:

> #### Slide 18

> The in-memory picture becomes:
> This example demonstrates how important the concept of reachability is.
> It’s obvious that John and Ann are still linked, both have incoming references. But that’s not enough.
> The former "family" object has been unlinked from the root, there’s no reference to it any more, so the whole island becomes unreachable and will be removed.

> #### Slide 19

> The basic garbage collection algorithm is called “mark-and-sweep”.
> The following “garbage collection” steps are regularly performed:
> * The garbage collector takes roots and “marks” (remembers) them.
> * Then it visits and “marks” all references from them.
> * Then it visits marked objects and marks their references. All visited objects are remembered, so as not to visit the same object twice in the future.
> * …And so on until every reachable (from the roots) references are visited.
> * All objects except marked ones are removed.

> #### Slide 20

> For instance, let our object structure look like this:
> We can clearly see an “unreachable island” to the right side. Now let’s see how “mark-and-sweep” garbage collector deals with it.

> #### Slide 21

> The first step marks the roots:

> #### Slide 22

> Then their references are marked:

> #### Slide 23

> …And their references, while possible:

> #### Slide 24

> Now the objects that could not be visited in the process are considered unreachable and will be removed:
> We can also imagine the process as spilling a huge bucket of paint from the roots, that flows through all references and marks all reachable objects. The unmarked ones are then removed.
> That’s the concept of how garbage collection works. JavaScript engines apply many optimizations to make it run faster and not affect the execution.

> #### Slide 25

> Some of the optimizations:

> * ***Generational collection*** – objects are split into two sets: “new ones” and “old ones”. Many objects appear, do their job and die fast, they can be cleaned up aggressively. Those that survive for long enough, become “old” and are examined less often.
> * ***Incremental collection*** – if there are many objects, and we try to walk and mark the whole object set at once, it may take some time and introduce visible delays in the execution. So the engine tries to split the garbage collection into pieces. Then the pieces are executed one by one, separately. That requires some extra bookkeeping between them to track changes, but we have many tiny delays instead of a big one.
> * ***Idle-time collection*** – the garbage collector tries to run only while the CPU is idle, to reduce the possible effect on the execution.

> #### Slide 26

> In conclusion, let me sum up my main points.
> The main things to know:
> * Garbage collection is performed automatically. We cannot force or prevent it.
> * Objects are retained in memory while they are reachable.
> * Being referenced is not the same as being reachable (from a root): a pack of interlinked objects can become unreachable as a whole.
> Modern engines implement advanced algorithms of garbage collection.
> In-depth knowledge of engines is good when you need low-level optimizations. It would be wise to plan that as the next step after you’re familiar with the language.

> #### Slide 27

> It was a little introduction to garbage collection since this is a big topic. I told you the basic information and showed the basic examples. Anyway,  I hope you enjoyed this presentation, thank you for your attention. !Don't read this => (Now I am happy to answer any questions you might have.)