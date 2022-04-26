There are of course drawbacks to the state machine pattern. First of all the phenomenon called “state explosion” is common as your systems grow in complexity. This simply means that you end up with so many states that the clarity that the FSM pattern provides is overshadowed by a myriad of states, making it hard to navigate and understand. Secondly its quite easy to end up in situations where you need to write duplicated code in your different states. E.g. in a game a “RunState” and a “JumpState” might need to perform similar logic in terms of camera movement. That logic would then to be duplicated in both states to some degree. Both of these problems can be solved *ish* by a slightly different state machine pattern, the hierarchical finite state machine.


The hierarchical finite state machine (HFSM) not only divides your system into separate states, it puts them into a hierarchy of sub-states, which themselves can be state machines. The problematic example above could then be divided into a parent state (“MoveState”) and two substates (“RunState” and “JumpState”). The camera logic that had to be duplicated can then reside in the parent state, and the specifics for running and jumping resides in the substates (Image 1).

![image](https://user-images.githubusercontent.com/65457437/164276259-49989d2d-a088-4eba-b9f3-d75dfe3be1de.png)

**States and substates**

The statemachine class shown above is a state in itself. To create a states you inherit from it and override the methods you need. The available ones “out of the box” are OnEnter(), OnUpdate() and OnExit(). 

The first substate you add will be the default substate. So in the example above the “runState” would be the default substate of “moveState”.
You must start the statemachine by using the method EnterStateMachine() on the root state (moveState in the example above). The OnEnter() methods will then be called hierarchically, from the root state to the deepest substate. You can then call the UpdateStateMachine() method on the root state. The OnUpdate() methods will then be called hierarchically aswell. The same principle goes for ExitStateMachine(), except that the OnExit() methods are called from the bottom up, starting with the lowest current substate.

**Transitions**

To create a transition from one sub state to another you use the AddTransition() method
