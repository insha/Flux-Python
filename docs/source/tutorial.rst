Tutorial
========

Here's a simple example of using the Flux state machine.

Once you have Flux installed (see :ref:`installation`), we can start by
importing in the components that we will need::

    import time

    from flux.machine import StateMachine
    from flux.state import State, StateInfo
    from flux.event import Event, EventInfo
    from flux.transition import Transition
    from flux.errors import StateMachineError, StateMachineEventError

Next step is to define the callbacks that will be triggered moving in and out of states::

    def did_enter_state(transition):
        print(f'==> FSM::Did enter {transition.destination_state}')

    def did_exit_state(transition):
        print(f'<== FSM::Did exit {transition.source_state}')

    def will_enter_state(transition):
        print(f'--> FSM::Will enter {transition.destination_state} from {transition.source_state}')

    def will_exit_state(transition):
        print(f'<-- FSM::Will exit {transition.source_state}')

And now we define the callbacks that our events will trigger::

    def did_fire_event(transition):
        print(f'=> FSM::Did fire {transition.event} in state {transition.source_state}')

    def will_fire_event(transition):
        print(f'=> FSM::Will fire {transition.event} in state {transition.source_state}')

Let's define our states first::

    waiting = State(name='waiting', info=StateInfo(did_enter_state=did_enter_state, 
                                                   did_exit_state=did_exit_state,
                                                   will_enter_state=will_enter_state,
                                                   will_exit_state=will_exit_state))

    filling = State(name='filling', info=StateInfo(did_enter_state=did_enter_state, 
                                                   did_exit_state=did_exit_state,
                                                   will_enter_state=will_enter_state,
                                                   will_exit_state=will_exit_state))

    done    = State(name='done', info=StateInfo(did_enter_state=did_enter_state, 
                                                did_exit_state=did_exit_state,
                                                will_enter_state=will_enter_state,
                                                will_exit_state=will_exit_state))

And now we can define our events and associate them to the states that we defined earlier::

    start_filling = Event(name='start_filling', info=EventInfo(source_states=[waiting], 
                                                               destination_state=filling,
                                                               will_fire_event=will_fire_event,
                                                               did_fire_event=did_fire_event))

    bottle_full   = Event(name='bottle_full', info=EventInfo(source_states=[filling], 
                                                             destination_state=done,
                                                             will_fire_event=will_fire_event,
                                                             did_fire_event=did_fire_event))

    remove_bottle = Event(name='remove_bottle', info=EventInfo(source_states=[done], 
                                                               destination_state=waiting,
                                                               will_fire_event=will_fire_event,
                                                               did_fire_event=did_fire_event))

Now we are ready to use our water bottle filling state machine::

    try:
        fsm = StateMachine(states=[waiting, filling, done], 
                           events=[start_filling, bottle_full, remove_bottle], 
                           initial_state=waiting)
    
        print('Adding a bottle to fill.')
        fsm.activate()
    
        time.sleep(1.0)
        print('')
        fsm.fire_event(start_filling)
        time.sleep(1.0)
        print('')
        fsm.fire_event(bottle_full)
        print('')
        fsm.fire_event(remove_bottle)
        print('✨ 🍰 ✨')
    except StateMachineEventError as e:
        print(e.message)
    except StateMachineError as e:
        print(e.message)
