0.11.1
------
* 'exclusive' class method marks methods as always exclusive and runs them
  outside of a Fiber (useful if you need more stack than Fibers provide)
* Celluloid::PoolManager returns its own class when #class is called, instead
  of proxying to a cell/actor in the pool.
* #receive now handles SystemEvents internally
* Celluloid::Timers extracted into the timers gem, which Celluloid now
  uses for its own timers

0.11.0
------
* Celluloid::Application constant permanently removed
* Celluloid::Pool removed in favor of Celluloid.pool
* Celluloid::Group renamed to Celluloid::SupervisionGroup, old name is
  still available and has not been deprecated
* Celluloid::ThreadPool renamed to Celluloid::InternalPool to emphasize its
  internalness
* Support for asynchronously calling private methods inside actors
* Future is now an instance method on all actors
* Async call exception logs now contain the failed method
* MyActor#async makes async calls for those who dislike the predicate syntax
* abort can now accept a string instead of an exception object and will raise
  RuntimeError in the caller's context

0.10.0
------
* Celluloid::Actor.current is now the de facto way to obtain the current actor
* #terminate now uses system messages, making termination take priority over
  other pending methods
* #terminate! provides asynchronous termination

0.9.1
-----
* Recurring timers with Celluloid#every(n) { ... }
* Obtain UUIDs with Celluloid.uuid
* Obtain the number of CPU cores available with Celluloid.cores
* Celluloid::Pool defaults to one actor per CPU core max by default

0.9.0
-----
* Celluloid::Pool supervises pools of actors
* Graceful shutdown which calls #terminate on all actors
* Celluloid::Actor.all returns all running actors
* Celluloid#exclusive runs a high priority block which prevents other methods
  from executing
* Celluloid.exception_handler { |ex| ... } defines a callback for notifying
  exceptions (for use with Airbrake, exception_notifier, etc.)

0.8.0
-----
* Celluloid::Application is now Celluloid::Group
* Futures no longer use a thread unless created with a block
* No more future thread-leaks! Future threads auto-terminate now
* Rename Celluloid#async to Celluloid#defer
* Celluloid#tasks now returns an array of tasks with a #status attribute
* Reduce coupling between Celluloid and DCell. Breaks compatibility with
  earlier versions of DCell.
* Celluloid::FSMs are no longer actors themselves
* Benchmarks using benchmark_suite

0.7.2
-----
* Workaround fiber problems on JRuby 1.6.5.1 in addition to 1.6.5
* Fix class displayed when inspecting dead actors

0.7.1
-----
* More examples!
* Cancel all pending tasks when actors crash
* Log all errors that occur during signaling failures
* Work around thread-local issues on rbx (see 52325ecd)

0.7.0
-----
* Celluloid::Task abstraction replaces Celluloid::Fiber
* Celluloid#tasks API to introspect on running tasks
* Move Celluloid::IO into its own gem, celluloid-io
* Finite state machines with Celluloid::FSM
* Fix bugs in supervisors handling actors that crash during initialize
* Old syntax Celluloid::Future() { ... } deprecated. Please use the #future
  method or Celluloid::Future.new { ... } to create futures
* New timer subsystem! Bullet point-by-bullet point details below
* Celluloid#after registers a callback to fire after a given time interval
* Celluloid.sleep and Celluloid#sleep let an actor continue processing messages
* Celluloid.receive and Celluloid#receive now accept an optional timeout
* Celluloid::Mailbox#receive now accepts an optional timeout

0.6.2
-----
* List all registered actors with Celluloid::Actor.registered
* All logging now handled through Celluloid::Logger
* Rescue DeadActorError in Celluloid::ActorProxy#inspect

0.6.1
-----
* Celluloid#links obtains Celluloid::Links for a given actor
* The #class method is now proxied to actors
* Celluloid::Fiber replaces the Celluloid.fiber and Celluloid.resume_fiber API
* Use Thread.mailbox instead of Thread.current.mailbox to obtain the mailbox
  for the current thread

0.6.0
-----
* Celluloid::Application classes for describing the structure of applications
  built with Celluloid
* Methods of actors can now participate in the actor protocol directly via
  Celluloid#receive
* Configure custom mailbox types using Celluloid.use_mailbox
* Define a custom finalizer for an actor by defining MyActor#finalize
* Actor.call and Actor.async API for making direct calls to mailboxes
* Fix bugs in Celluloid::Supervisors which would crash on startup if the actor
  they're supervising also crashes on startup
* Add Celluloid.fiber and Celluloid.resume_fiber to allow extension APIs to
  participate in the Celluloid fiber protocol

0.5.0
-----
* "include Celluloid::Actor" no longer supported. Use "include Celluloid"
* New Celluloid::IO module for actors that multiplex IO operations
* Major overhaul of Celluloid::Actor internals (see 25e22cc1)
* Actor threads are pooled in Celluloid::Actor::Pool, improving the speed
  of creating short-lived actors by over 2X
* Classes that include Celluloid now have a #current_actor instance method
* Celluloid#async allows actors to make indefinitely blocking calls while
  still responding to messages
* Fix a potential thread safety bug in Thread#mailbox
* Experimental Celluloid::TCPServer for people wanting to write servers in
  Celluloid. This may wind up in another gem, so use at your own risk!
* Magically skip ahead a few version numbers to impart the magnitude of this
  release. It's my versioning scheme and I can do what I wanna.

0.2.2
-----

* AbortErrors now reraise in caller scope and get a caller-focused backtrace
* Log failed async calls instead of just letting them fail silently
* Properly handle arity of synchronous calls
* Actors can now make async calls to themselves
* Resolve crashes that occur when sending responses to exited/dead callers

0.2.1
-----

* Hack around a bug of an indeterminate cause (2baba3d2)
* COLON!#@!

0.2.0
-----

* Support for future method calls with MyActor#future
* Initial signaling support via MyActor#signal and MyActor#wait
* Just "include Celluloid" works in lieu of "include Celluloid::Actor"
* Futures terminate implicitly when their values are obtained
* Add an underscore prefix to all of Celluloid's instance variables so they don't
  clash with user-defined ones.

0.1.0
-----
* Fiber-based reentrant actors. Requires Ruby 1.9
* MyActor.new (where MyActor includes Celluloid::Actor) is now identical to .spawn
* Terminate actors with MyActor#terminate
* Obtain current actor with Celluloid.current_actor
* Configurable logger with Celluloid.logger
* Synchronization now based on ConditionVariables instead of Celluloid::Waker
* Determine if you're in actor scope with Celluloid.actor?

0.0.3
-----
* Remove self-referential dependency in gemspec

0.0.1
-----
* Initial release
