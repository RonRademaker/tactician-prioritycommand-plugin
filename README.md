# tactician-prioritycommand-plugin
Tactician plugin that allows adding a priority to a command which influences when and in what order commands will be executed

[![Build Status](https://travis-ci.org/RonRademaker/tactician-prioritycommand-plugin.svg?branch=master)](https://travis-ci.org/RonRademaker/tactician-prioritycommand-plugin)
[![SensioLabsInsight](https://insight.sensiolabs.com/projects/3e8f0f6d-43d3-4761-ae75-14461264b8df/mini.png)](https://insight.sensiolabs.com/projects/3e8f0f6d-43d3-4761-ae75-14461264b8df)

# Concept
The plugin adds Middleware that allows you to prioritize your commands. To give priority to a command let is extend from one of the command implementations in this library:

Default supported command types:
- AbstractFreeCommand (bus, you may take a detour)
- AbstractRequestCommand (bus, you may take a detour, but you may not start a new round)
- AbstractSequenceCommand (bus, you may take a detour, but nobody gets off before I do)
- AbstractUrgentCommand (take me to my destination asap)

You can create an interface to your event dispatcher (one for symfony ships with this library). You should attach the PriorityMiddleware::REQUEST queue to some event you always dispatch, preferably after output is sent to your users.
Similar you can create an interface to some messaging / queueing system (I don't use one yet so I haven't created any yet) for the PriorityMiddleware::FREE queue.

# Suggested priorities
Obviously you're free to give your commands any priority you like, but these guidelines may help:

- Urgent: anything that affects the output you send to your user OR anything that affects the behavior of incoming request
- Request: anything that affects any subsequent requests from your user
- Sequence: anything that affects the behavior of following commands (i.e. because it sets an id in your database something else depends on)
- Free: everything you need to get done but it doesn't matter much when
