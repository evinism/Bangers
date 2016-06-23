# Bangers
A hierarchical state management solution!

Bangers steals many patterns from React/Redux and it's implementation at Procore. Ideas for making Bangers is based around some ideas for composition, some ideas from prototypal inheritance, but mostly it's built as a solution to several problems a hierarchical state may be able to solve:

 - State schema is often archaic and cumbersome, and split between several files.
 - Selectors require props to be able to read portions of the state, tying the view layer pretty heavily to the state management layer.
 - Actions require the passing of scoping information to affect something deep in state.
 - Higher Order Components get old very very quickly, and lead to MASSIVE call stacks.
 
These are all seemingly problems with the comingling of State and View, which is the exact problem redux was trying to solve.
 
### Some quick descriptions of where I hope for CVS to go, and thus what Bangers should implement natively.

Let type `plug` be an object of shape `{props: {}, actions: {}}` that implements `observable`. Plugs should be the main interface between Bangers and any other object out there.

Let values within `props` be immutable data structures, arrays, objects, primitives, etc.

Let values within `actions` be any action of form (payload) => (), a function whose sole side-effect should be to dispatch a state transition. (hmm maybe we can extend this such that any side effects can be given? No that's dumb because it ties signal layer to view layer? Maybe not, as this ruins the abstraction)

A set of `plug` objects will serve as the interface between state management and the view layer. 

This leads to a quintessential abstraction: Any state management solution implementing CVS must act as a state machine with no side effects, and Bangers will follow suit..

### Back to Bangers

Bangers in it's default interface will provide a non-scoped callback structure in the form of an observable object with `plug`. IE, callbacks are by default made to the instance in question rather than to a global state within which a scope resides. A state wrapper can be provided such that scope refers to instance, and this is the Scoped middleware that will be provided. This will be of form `scope => plug`. I will discuss scopes later.

Let a Bangers Module be a thing you don't have to care about the structure of, ever. Because that would be silly if you had to care about it. So basically, as far as you're concerned, let it be a function:

(Props) => Instance

Each instance of a Bangers module has it's own immutable state, and can call the module's parent's state reducers. Can we make this work on the state as a whole, somehow? AKA:
```
state => compose(
  modifyLocalState,
  modifyParentState,
  modifyLocalState,
);
```
This is how composition should work in Bangers, if it's gonna work ever. Let's solve that problem too.

This means the application as a whole has immutable state, and that any action on any module can be viewed as a quick dispatch of an action.

### Advantages:
The main advantage I think this has over pure OO is the Extreme Cousin case.
Let's say A and B are distant cousins with common ancestor C, but we want A and B to communicate. The Redux way is to allow actions to be dispatched to one another with a global action/reducer paradigm. Which is dope I guess. But if I want them to  share the same state, I have to go ALL the way to common ancestor C, or introduce 

But not in Bangers!

```

```

This is allowed because inheritance is not a classic IS A relationship, nor should it be. 

"But wait, diamonds!"

Yep, that's a problem. Hopefully, if I'm clever I can make it so that they all inherit the same prototype functions. No issue!

To be continued...
