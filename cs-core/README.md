# Computer Science Fundamentals — Interview Questions and Answers

> A placement-focused CS Fundamentals question bank for **SDE-1 / Backend / Software Engineering interviews**.
>
> Designed in the **question-first, topic-wise README style** of the uploaded ML/DL interview guide, with short interview-ready answers, targeted follow-ups, and SQL practice.

**Modules:** Database Management Systems (DBMS) · Object-Oriented Programming (OOP in C++) · Operating Systems (OS) · Computer Networks (CN) · SQL

**Target depth:** fundamentals → intermediate interviews. Advanced distributed-systems topics appear only when they naturally follow a core CS concept.

## Table of Contents

- [1. Database Management Systems (DBMS)](#1-database-management-systems-dbms)
- [2. Object-Oriented Programming (OOP in C++)](#2-object-oriented-programming-oop-in-c)
- [3. Operating Systems (OS)](#3-operating-systems-os)
- [4. Computer Networks (CN)](#4-computer-networks-cn)
- [5. Structured Query Language (SQL)](#5-structured-query-language-sql)
- [6. High-Yield Scenario Questions](#6-high-yield-scenario-questions)
- [7. Final Revision Checklist](#7-final-revision-checklist)
- [Further Reading](#further-reading)

### How to use this README

1. **Read the question before the answer.** Try to answer aloud in 30–60 seconds.
2. Aim for **definition → why it matters → one example or trade-off**. Answers here are intentionally concise revision notes, not full lectures.
3. For **OOP**, be ready to illustrate answers with C++; for **SQL**, write the query without looking at the solution.
4. Prioritize fundamental questions before implementation details. Questions labeled **[Stretch]** are useful follow-ups, not prerequisites.
5. Don't memorize exact wording: be able to explain what happens in a real application.

---

## 1. Database Management Systems (DBMS)

### A. Fundamentals, schema and relationships

- **What is a DBMS? How is it different from a file system?**
  - **Answer:** A DBMS manages structured data with querying, constraints, concurrent access and recovery. Plain files lack these built-in guarantees.
- **What is the difference between DBMS and RDBMS?**
  - **Answer:** An RDBMS represents data as related tables and generally enforces relational constraints; DBMS is the broader category.
- **What are schema and database instance?**
  - **Answer:** Schema is the database's logical design; instance is the actual data stored at a given moment.
- **Explain the three-level database architecture and data independence.**
  - **Answer:** External (views), conceptual (logical schema), internal (physical storage). Data independence lets one level change with minimal impact on others.
- **What are rows, columns, relations, tuples and attributes?**
  - **Answer:** In the relational model: relation ≈ table, tuple ≈ row, attribute ≈ column. A row describes one record.
- **What are primary, candidate, alternate, super and foreign keys?**
  - **Answer:** Candidate keys minimally identify a row; one is chosen as primary, remaining are alternate. Any superset of a candidate key is a superkey. A foreign key references a candidate/unique key in another table.
- **What is a composite key? When would you use it?**
  - **Answer:** A key formed from multiple columns, such as `(student_id, course_id)` in an enrollment table.
- **How do PRIMARY KEY, UNIQUE, NOT NULL, CHECK and FOREIGN KEY differ?**
  - **Answer:** Primary key identifies rows and disallows NULL; UNIQUE prevents duplicates (NULL behavior varies by DB); NOT NULL requires a value; CHECK enforces a predicate; foreign key enforces referential integrity.
- **What is referential integrity? What are ON DELETE CASCADE and SET NULL?**
  - **Answer:** References must point to valid parent rows. CASCADE deletes dependent rows; SET NULL clears the referencing field where allowed.
- **What are one-to-one, one-to-many and many-to-many relationships?**
  - **Answer:** One profile per user; many orders per user; many students per course. Many-to-many typically needs a junction table.
- **What are entities, attributes and relationships in an ER model?**
  - **Answer:** Entities represent things, attributes describe them, and relationships connect them. The ER model guides relational table design.
- **What is a view? How is it different from a table?**
  - **Answer:** A regular view is a stored query whose result is computed when used; a table stores data. A materialized view stores a refreshed result.

### B. Functional dependencies and normalization

- **What is a functional dependency?**
  - **Answer:** `X → Y` means any two rows agreeing on X must agree on Y; e.g. `employee_id → employee_name`.
- **Why do we normalize tables?**
  - **Answer:** To reduce duplication and insertion, update and deletion anomalies while preserving meaningful relationships.
- **What are insertion, update and deletion anomalies?**
  - **Answer:** Insertion may require unrelated data, updates may leave contradictory copies, and deletion may accidentally remove independent facts.
- **Explain 1NF, 2NF and 3NF with the key difference.**
  - **Answer:** 1NF uses atomic values; 2NF eliminates partial dependency of non-prime attributes on a candidate key; 3NF eliminates problematic transitive dependencies (formally, for every `X→A`, X is a superkey or A is prime).
- **What is BCNF? How is it stricter than 3NF?**
  - **Answer:** Every nontrivial functional dependency `X→Y` must have X as a superkey. 3NF has an exception when the dependent attribute is prime.
- **What is a partial dependency?**
  - **Answer:** A non-prime attribute depends on a proper subset of a composite candidate key; a 2NF violation.
- **What is a transitive dependency?**
  - **Answer:** A dependency such as `student_id → department_id → department_name`; storing all three in one table can create duplication.
- **What are lossless decomposition and dependency preservation?**
  - **Answer:** Lossless decomposition allows the original relation to be reconstructed by joining the decomposed tables without spurious rows. Dependency preservation allows enforcing original dependencies without joins.
- **When is denormalization useful?**
  - **Answer:** When extra redundancy, aggregates or precomputed data materially improve read performance and the application can maintain consistency.
- **[Stretch] What is 4NF?**
  - **Answer:** A stronger normal form concerned with independent multivalued dependencies; every nontrivial multivalued dependency must have a superkey determinant.

### C. Transactions, ACID and isolation

- **What is a transaction?**
  - **Answer:** A logically grouped unit of database operations that either commits or rolls back.
- **Explain the ACID properties.**
  - **Answer:** Atomicity: all-or-nothing; Consistency: constraints preserved; Isolation: concurrent transactions behave according to the chosen isolation guarantee; Durability: committed changes survive failures.
- **What are COMMIT, ROLLBACK and SAVEPOINT?**
  - **Answer:** COMMIT makes changes permanent; ROLLBACK cancels uncommitted work; SAVEPOINT allows partial rollback within a transaction.
- **What is a schedule? What is serial vs concurrent execution?**
  - **Answer:** A schedule is the order of operations from transactions. Serial runs transactions one after another; concurrent interleaves them.
- **What is conflict serializability? How do you check it?**
  - **Answer:** A concurrent schedule is conflict-serializable if it can be transformed into a serial one by swapping nonconflicting operations. An acyclic precedence graph is the test.
- **What is a dirty read?**
  - **Answer:** Reading another transaction's uncommitted data, which may later be rolled back.
- **What is a non-repeatable read?**
  - **Answer:** Reading the same row twice and observing different committed values because another transaction updated it.
- **What is a phantom read?**
  - **Answer:** Repeating a predicate query and finding a changed set of matching rows because another transaction inserted, deleted or changed matching rows.
- **What is a lost update?**
  - **Answer:** Two transactions update based on an earlier value and one overwrites the other's change without coordination.
- **Explain READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ and SERIALIZABLE.**
  - **Answer:** They provide increasingly strong isolation in the SQL standard. Implementation details vary by engine; SERIALIZABLE should behave as some serial ordering of committed transactions.
- **What are shared and exclusive locks?**
  - **Answer:** Shared locks allow compatible readers; exclusive locks protect writes against conflicting reads/writes in lock-based engines.
- **What is two-phase locking (2PL)?**
  - **Answer:** A transaction first acquires locks (growing phase), then releases locks (shrinking phase). Strict 2PL holds exclusive locks until commit/abort, avoiding cascading aborts.
- **What is MVCC and why is it useful?**
  - **Answer:** Multi-Version Concurrency Control keeps row versions so many readers can access a consistent snapshot without blocking ordinary writers.
- **What are optimistic and pessimistic concurrency control?**
  - **Answer:** Optimistic allows concurrent work then checks for conflicts; pessimistic locks before potentially conflicting operations.
- **What is database deadlock? How is it resolved?**
  - **Answer:** Transactions wait cyclically for one another's locks. The engine detects/prevents deadlocks and typically aborts a victim transaction to break the cycle.
- **How would you avoid overselling the last item in inventory?**
  - **Answer:** Use an atomic conditional update within a transaction, e.g. `UPDATE stock SET qty=qty-1 WHERE id=? AND qty>0`, and verify one row was affected; coordinate order creation in the same transaction.

### D. Indexing, storage and query optimization

- **What is an index? Why can it make queries faster?**
  - **Answer:** An auxiliary structure maps search keys to rows, reducing work for selective predicates and ordered access at the cost of storage and write overhead.
- **What is the difference between B-tree and B+ tree indexes?**
  - **Answer:** Both are balanced search structures. B+ trees store data entries at leaves and link leaves for efficient range scans; internal nodes guide traversal.
- **Clustered vs non-clustered index: what is the conceptual difference?**
  - **Answer:** Clustered organization determines how table data is stored/ordered by key; secondary/non-clustered indexes are separate lookup structures. Exact details depend on DBMS.
- **What is a composite index? What is the leftmost-prefix idea?**
  - **Answer:** A composite index keys on `(a,b,...)`. Many B-tree engines use it efficiently for filters starting from leading indexed columns; range predicates affect later column usefulness.
- **What is a covering index?**
  - **Answer:** An index containing all columns needed for a query, potentially avoiding extra table lookups.
- **Why doesn't the database always use an index?**
  - **Answer:** Full scans can be cheaper on small tables or low-selectivity queries; functions, data distribution and index structure also affect choices.
- **How do indexes affect INSERT, UPDATE and DELETE?**
  - **Answer:** Writes must also maintain relevant indexes, increasing write cost and storage.
- **What is the difference between a full table scan and an index scan?**
  - **Answer:** A full scan reads many/all table pages; an index scan uses index entries to locate matching rows, potentially with extra random I/O.
- **What is `EXPLAIN` / an execution plan?**
  - **Answer:** A report of the optimizer's chosen operations: scans, joins, sorts, estimated cost and (when executed) actual row counts/timing.
- **What are nested-loop, hash and sort-merge joins?**
  - **Answer:** Nested loop probes matches repeatedly; hash join hashes one input; merge join combines sorted inputs. The optimizer chooses based on data sizes, indexes and ordering.
- **What is database selectivity?**
  - **Answer:** The fraction of rows that match a predicate. Highly selective predicates often benefit from indexes.
- **What is write-ahead logging (WAL)?**
  - **Answer:** Log changes are made durable before corresponding data pages are flushed, enabling redo/undo-style recovery after a crash.

### E. Scaling and production follow-ups

- **What is replication? How is it different from sharding?**
  - **Answer:** Replication copies the same dataset to other nodes for availability/read scale; sharding partitions different subsets across nodes for data/write scale.
- **What is horizontal vs vertical scaling for a database?**
  - **Answer:** Vertical means bigger resources for one machine; horizontal means distributing work/data across multiple machines.
- **SQL vs NoSQL: how would you choose?**
  - **Answer:** Choose based on relationships, query patterns, transactions, access scale and operational needs; neither is universally faster or more scalable.
- **What is connection pooling? Why is it important?**
  - **Answer:** Reuse a bounded set of database connections to avoid repeated setup and protect the server from excessive concurrent connections.
- **What is an N+1 query problem?**
  - **Answer:** One query fetches N objects, then N more queries fetch their related data. Fix using joins, eager loading or batching.
- **What is the CAP theorem? [Stretch]**
  - **Answer:** When a network partition occurs, a distributed system cannot guarantee both linearizable consistency and availability for every request; CAP is not a blanket choice between three independent features.
- **When would you cache database reads, and what is cache invalidation?**
  - **Answer:** Cache frequently read data to reduce latency/load. On changes, expire/update cached values or accept bounded staleness with TTLs.
- **What is a database backup vs a replica?**
  - **Answer:** A backup preserves recoverable historical data; a replica mirrors ongoing writes and may replicate accidental deletions, so it is not a substitute for backups.

---

## 2. Object-Oriented Programming (OOP in C++)

### A. OOP foundations

- **What is object-oriented programming?**
  - **Answer:** A style of modeling software using objects that bundle state and behavior; useful for abstraction and modularity.
- **What is the difference between a class and an object?**
  - **Answer:** A class defines a type and its behavior; an object is an instance of that type with its own state.
- **Explain the four pillars of OOP.**
  - **Answer:** Encapsulation, abstraction, inheritance and polymorphism.
- **Encapsulation vs abstraction: what is the difference?**
  - **Answer:** Encapsulation bundles data and controls access to it; abstraction exposes essential behavior while hiding implementation complexity.
- **How do `public`, `private` and `protected` differ in C++?**
  - **Answer:** Public members are accessible through the interface; private members are accessible inside the class/friends; protected members are also accessible in derived classes.
- **What is inheritance, and when should it be used?**
  - **Answer:** A derived type reuses or refines a base type's interface/behavior when an actual *is-a* relationship exists.
- **What are single, multilevel, multiple and hierarchical inheritance?**
  - **Answer:** One parent; parent→child→grandchild; several parents for one child; and one parent with multiple children, respectively.
- **Inheritance vs composition: which is usually preferable?**
  - **Answer:** Composition models *has-a* relationships and usually creates looser coupling; use inheritance when substitutability is genuinely valid.
- **What is association vs aggregation vs composition?**
  - **Answer:** Association is a general relationship; aggregation is shared/weak ownership; composition represents stronger ownership and aligned lifetime in the model.
- **What is a constructor? What constructor types should you know?**
  - **Answer:** A special function initializing objects; know default, parameterized, copy and move constructors, and constructor delegation.
- **What is a destructor? When does it run?**
  - **Answer:** It cleans up when an object lifetime ends (or is deleted); automatic objects are destroyed when their scope ends.
- **What is constructor/destructor execution order in inheritance?**
  - **Answer:** Base class construction precedes member/derived body; destruction happens in reverse, derived before base. Member initialization follows declaration order.

### B. Polymorphism, inheritance and dispatch

- **What is compile-time vs run-time polymorphism?**
  - **Answer:** Compile-time: overloads/templates resolved at compilation. Run-time: virtual function calls dispatched through a base reference/pointer based on the actual object's type.
- **What is function overloading vs overriding?**
  - **Answer:** Overloading defines same-name functions with different parameters; overriding replaces a virtual base-class function in a derived class.
- **Can functions be overloaded using only different return types?**
  - **Answer:** No. A different parameter list (or applicable qualifiers) is required.
- **What is a virtual function?**
  - **Answer:** A base-class member declared `virtual` that enables dynamic dispatch when called through base pointers/references.
- **What is a pure virtual function?**
  - **Answer:** A virtual function declared `= 0`; it makes the class abstract. It may still have a separately defined implementation.
- **What is an abstract class? Can you instantiate one?**
  - **Answer:** A class with at least one final-overrider pure virtual function; it cannot be instantiated directly.
- **What is an interface in C++?**
  - **Answer:** Usually an abstract class exposing pure virtual operations, often with a public virtual destructor.
- **What are a vtable and vptr?**
  - **Answer:** Common compiler mechanisms for virtual dispatch; exact layout is implementation-specific and not mandated by the C++ standard.
- **Why do base classes need a virtual destructor?**
  - **Answer:** If an object may be deleted through a base pointer, a public virtual destructor ensures the derived destructor runs correctly.
- **What is object slicing?**
  - **Answer:** Assigning/passing a derived object by value as its base type copies only the base subobject, losing derived-specific state.
- **What is the diamond problem in multiple inheritance?**
  - **Answer:** Two branches inherit a common base, creating ambiguity/duplicate base subobjects; virtual inheritance can share a single base subobject.
- **What is virtual inheritance?**
  - **Answer:** A mechanism that ensures one shared virtual base subobject in diamond-shaped inheritance.
- **What do `override` and `final` mean?**
  - **Answer:** `override` makes the compiler verify a method actually overrides a virtual base method; `final` forbids further overriding or deriving, depending on placement.
- **Can a constructor be virtual? Can a destructor be virtual?**
  - **Answer:** Constructors cannot be virtual; destructors can and often should be virtual in polymorphic base classes.
- **What is static binding vs dynamic binding?**
  - **Answer:** Static binding determines the called function at compile time; dynamic binding selects the final overrider at runtime for virtual calls.
- **What happens if you call a virtual function inside a constructor or destructor?**
  - **Answer:** Dispatch is limited to the class currently being constructed/destroyed; it does not dispatch to more-derived classes not yet constructed or already destroyed.

### C. Copying, moving and object lifetime

- **What is shallow copy vs deep copy?**
  - **Answer:** Shallow copy duplicates member values (including raw pointer addresses). Deep copy duplicates owned pointed-to resources as well.
- **What is the Rule of Three?**
  - **Answer:** A class managing resources that needs a custom destructor, copy constructor or copy assignment often needs all three.
- **What is the Rule of Five?**
  - **Answer:** Extends resource-management special members to move constructor and move assignment.
- **What is the Rule of Zero?**
  - **Answer:** Prefer resource-owning standard types and avoid explicitly managing special member functions unless necessary.
- **What is a copy constructor vs copy assignment operator?**
  - **Answer:** Copy construction creates a new object from another; copy assignment changes an already existing object.
- **What is a move constructor? Why is it useful?**
  - **Answer:** Initializes an object by transferring resources from an rvalue, avoiding expensive deep copies when possible.
- **What are lvalues and rvalues?**
  - **Answer:** An lvalue identifies a persistent object/location; an rvalue is typically a temporary/value suitable for moving from, subject to C++ value-category rules.
- **What does `std::move` actually do?**
  - **Answer:** It casts its argument to an rvalue reference; it does not move anything by itself. A move constructor/assignment performs the transfer.
- **What is the difference between passing by value, reference and const reference?**
  - **Answer:** By value copies/moves; by reference allows modifying the caller's object; `const&` avoids a copy for large objects while preventing mutation through that reference.
- **What are stack and heap allocations in C++?**
  - **Answer:** Local automatic objects generally have scope-bound storage; dynamic objects created with `new` live until deleted. C++ storage duration is the precise concept.
- **What is RAII?**
  - **Answer:** Resource Acquisition Is Initialization: tie resource ownership to object lifetime so destructors safely release files, locks, memory or sockets.
- **What is a memory leak? What is a dangling pointer?**
  - **Answer:** A leak loses the ability to free allocated memory; a dangling pointer refers to an object that no longer exists.
- **What is `nullptr` vs `NULL`?**
  - **Answer:** `nullptr` has type `std::nullptr_t` and behaves predictably in overload resolution; `NULL` is a legacy null-pointer constant often defined as `0`.
- **What is `new/delete` vs `malloc/free`?**
  - **Answer:** `new` constructs and `delete` destroys C++ objects; `malloc/free` manage raw memory and do not invoke constructors/destructors.
- **When would you use `unique_ptr`, `shared_ptr` and `weak_ptr`?**
  - **Answer:** `unique_ptr` for sole ownership, `shared_ptr` for shared ownership, and `weak_ptr` for non-owning observation and breaking reference cycles.
- **What is a reference cycle with `shared_ptr`?**
  - **Answer:** Two objects own each other through shared pointers so reference counts never reach zero; replace a back-reference with `weak_ptr`.

### D. Common C++ language and design interview questions

- **What is the difference between `struct` and `class` in C++?**
  - **Answer:** They have the same capabilities; default member/inheritance access is public for `struct` and private for `class`.
- **What are static data members and static member functions?**
  - **Answer:** Static data belongs to the class rather than each instance; static methods have no `this` pointer.
- **What is the `this` pointer?**
  - **Answer:** A pointer to the object on which a non-static member function is invoked.
- **What does `const` after a member function mean?**
  - **Answer:** It promises not to modify the object's non-`mutable` state through `this`; useful for read-only interfaces.
- **What is function/operator overloading? What operators cannot be overloaded?**
  - **Answer:** Defining several functions or operator behaviors for user types. Operators such as `.`, `::`, `?:` and `sizeof` cannot be overloaded.
- **What is exception safety? What is stack unwinding?**
  - **Answer:** Exception safety means maintaining promised invariants/resources if operations throw; stack unwinding destroys automatic objects as exceptions propagate.
- **What are the SOLID principles?**
  - **Answer:** Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation and Dependency Inversion—guidelines for maintainable interfaces and dependencies.
- **What is Liskov Substitution? Give a practical symptom of violating it.**
  - **Answer:** A derived type should work wherever its base contract is expected. A subtype that throws for an operation the base promises violates that expectation.
- **What is dependency injection?**
  - **Answer:** Supplying collaborators through constructors/parameters instead of constructing hard-coded dependencies inside a class; helps testing and decoupling.
- **What is the difference between an abstract class and a concrete class?**
  - **Answer:** An abstract class cannot be instantiated due to pure virtual functions; a concrete class supplies all required implementations.
- **What are templates? How are they different from run-time polymorphism?**
  - **Answer:** Templates generate type-specific code at compile time; virtual functions perform runtime dispatch through a common base interface.
- **What is a friend function or friend class?**
  - **Answer:** An explicitly named external function/class granted access to private/protected members; friendship is not automatically mutual or inherited.

### E. C++ mini implementation prompts

- **How would you demonstrate run-time polymorphism in C++?**
  - **Answer:** Define `virtual double area() const = 0` in `Shape`, override it in `Circle`, and invoke `area()` through `Shape&` or `Shape*`.
- **How would you prevent accidental copying of a resource owner?**
  - **Answer:** Use `std::unique_ptr` for ownership, or explicitly `= delete` the copy constructor and copy assignment.
- **How would you safely represent a `Car` owning an `Engine`?**
  - **Answer:** Prefer composition: `class Car { Engine engine; };` when ownership/lifetimes align; use `unique_ptr<Engine>` if dynamic lifetime is required.
- **How would you write a polymorphic base class suitable for deletion?**
  - **Answer:** Provide a public `virtual ~Base() = default;` when deleting objects through `Base*` is intended.

---

## 3. Operating Systems (OS)

### A. Processes, threads and system basics

- **What is an operating system? What are its core responsibilities?**
  - **Answer:** It manages CPU, memory, I/O, files, security and resource sharing, exposing abstractions to applications.
- **What is the difference between kernel space and user space?**
  - **Answer:** Kernel mode has privileged access to hardware/resources; user applications are restricted and request services through system calls.
- **What is a system call? Give examples.**
  - **Answer:** A controlled request to the kernel, e.g. `read`, `write`, `open`, `fork` or `mmap` on Unix-like systems.
- **What is a process vs a program?**
  - **Answer:** A program is executable code on disk; a process is an executing instance with address space, resources and state.
- **What are the common process states?**
  - **Answer:** New, ready, running, waiting/blocked and terminated (with possible additional suspended states).
- **What is a PCB (Process Control Block)?**
  - **Answer:** Kernel metadata about a process, including its identifier, state, registers, scheduling data and resource references.
- **What is a thread? How is it different from a process?**
  - **Answer:** Threads share a process's address space and many resources; processes normally have separate virtual address spaces.
- **Which resources do threads share, and which are per-thread?**
  - **Answer:** Threads share code, heap and open process resources; each has its own stack, register state and execution context.
- **What is context switching? Why is it expensive?**
  - **Answer:** Switching the executing task requires saving/restoring state and may disrupt CPU caches/TLB behavior.
- **What is `fork()` vs `exec()` vs `wait()`?**
  - **Answer:** `fork()` creates a child process, `exec()` replaces its program image, and `wait()` lets the parent collect a child's exit status.
- **What are zombie and orphan processes?**
  - **Answer:** A zombie has exited but its parent has not reaped its status; an orphan continues after its parent exits and is adopted by a designated reaper.
- **What is a daemon/service process?**
  - **Answer:** A background process serving requests or performing recurring system work.
- **What is the difference between concurrency and parallelism?**
  - **Answer:** Concurrency means tasks make progress during overlapping periods; parallelism means they run simultaneously on multiple execution units.
- **What is a CPU-bound vs I/O-bound task?**
  - **Answer:** CPU-bound work spends most time computing; I/O-bound work frequently waits for disks, networks or devices.
- **What is multitasking, multiprocessing and multithreading?**
  - **Answer:** Multitasking schedules multiple tasks; multiprocessing uses multiple CPUs/cores or processes; multithreading uses threads within a process.

### B. CPU scheduling

- **What is CPU scheduling? What metrics evaluate it?**
  - **Answer:** Selecting a ready task to run; metrics include turnaround, waiting and response time, throughput and CPU utilization.
- **What is preemptive vs non-preemptive scheduling?**
  - **Answer:** Preemptive scheduling can interrupt a running process; non-preemptive lets it run until blocking or completion/yield.
- **How does FCFS scheduling work? What is the convoy effect?**
  - **Answer:** First Come First Served runs by arrival order; a long job can delay many short jobs.
- **How does SJF scheduling work? What is its weakness?**
  - **Answer:** Shortest Job First selects the shortest next CPU burst; exact burst lengths are hard to know and long jobs may starve.
- **What is SRTF scheduling?**
  - **Answer:** Shortest Remaining Time First preempts the current job when a newly ready job has less remaining CPU time.
- **How does Round Robin scheduling work? Why does time quantum matter?**
  - **Answer:** Each task gets a bounded time slice. Tiny quanta cause switch overhead; huge quanta approach FCFS.
- **What is priority scheduling? What are starvation and aging?**
  - **Answer:** Higher-priority tasks run first; low-priority tasks may wait indefinitely. Aging gradually increases waiting tasks' priority.
- **What is multilevel feedback queue (MLFQ) scheduling? [Stretch]**
  - **Answer:** A scheduler with multiple priority queues that moves tasks based on observed CPU usage/waiting behavior to balance responsiveness and fairness.
- **How do you compute turnaround, waiting and response time?**
  - **Answer:** Turnaround = completion − arrival; waiting = turnaround − total CPU burst time (for simple CPU-only jobs); response = first run − arrival.
- **Why does a context-switch overhead affect throughput?**
  - **Answer:** Time spent switching tasks is time not spent running useful application instructions.

### C. Synchronization, race conditions and deadlocks

- **What is a race condition?**
  - **Answer:** Output depends on uncontrolled interleaving of concurrent operations; e.g. two threads incrementing a shared counter without coordination.
- **What is a critical section? What requirements should its solution meet?**
  - **Answer:** Code accessing shared state. Classical requirements: mutual exclusion, progress and bounded waiting.
- **What is a mutex?**
  - **Answer:** An ownership-based lock allowing only one thread at a time in a protected critical section.
- **What is a semaphore? Binary vs counting semaphore?**
  - **Answer:** A synchronization counter supporting wait/signal operations; binary has 0/1 permits, counting supports multiple permits.
- **Mutex vs semaphore: how do you decide?**
  - **Answer:** Use mutex for exclusive ownership of a critical section; semaphore for signaling or managing N identical permits.
- **What is a spinlock? When can it be useful?**
  - **Answer:** A lock that busy-waits rather than sleeping; sometimes suitable for extremely short critical sections when sleeping would cost more.
- **What is a condition variable? Why is it used with a mutex?**
  - **Answer:** It lets a thread sleep until a condition may have changed, while atomically releasing/reacquiring the associated lock around the wait.
- **Explain the producer-consumer problem.**
  - **Answer:** Producers add to a bounded buffer; consumers remove. Use synchronization to protect the queue and signal not-full/not-empty conditions.
- **Explain the readers-writers problem.**
  - **Answer:** Multiple readers may share access, but a writer needs exclusive access; implementations balance throughput and starvation risk.
- **What is a deadlock? List the four Coffman conditions.**
  - **Answer:** Permanent cyclic waiting for resources. Mutual exclusion, hold-and-wait, no preemption and circular wait are necessary conditions.
- **What is deadlock prevention vs avoidance vs detection/recovery?**
  - **Answer:** Prevention removes a necessary condition; avoidance grants only safe allocations; detection finds deadlocks then aborts/preempts work.
- **What is the Banker's algorithm? [Stretch]**
  - **Answer:** A deadlock-avoidance algorithm that checks whether granting a resource request leaves some safe completion sequence.
- **What is starvation vs deadlock vs livelock?**
  - **Answer:** Starvation: one task indefinitely misses progress; deadlock: tasks block each other; livelock: tasks remain active but make no useful progress.
- **What is priority inversion?**
  - **Answer:** A high-priority task waits on a lock held by a lower-priority task while medium-priority work delays lock release; priority inheritance can help.
- **How is a counting semaphore different from a condition variable?**
  - **Answer:** Semaphore retains permit count; a condition-variable notification is not stored as a persistent permit and requires checking a predicate under a mutex.

### D. Memory management and virtual memory

- **What is logical/virtual vs physical address?**
  - **Answer:** A process uses virtual addresses translated to physical memory addresses by hardware/OS mapping.
- **What is virtual memory and why is it useful?**
  - **Answer:** An abstraction giving each process a protected address space while allowing on-demand physical-memory allocation and disk-backed pages.
- **What is paging? What are pages and frames?**
  - **Answer:** Virtual memory is split into fixed-size pages and RAM into frames; page tables map virtual pages to frames.
- **What is a page table?**
  - **Answer:** A mapping from virtual pages to physical frames, with protection/presence and other status bits.
- **What is TLB and why does it improve performance?**
  - **Answer:** Translation Lookaside Buffer caches recent virtual-to-physical translations to reduce page-table walks.
- **What is a page fault?**
  - **Answer:** A memory access triggers a trap because the page needs OS intervention—e.g. load a nonresident valid page or reject an invalid access.
- **What is demand paging?**
  - **Answer:** Pages are loaded into physical memory as they are accessed rather than all up front.
- **What is internal vs external fragmentation?**
  - **Answer:** Internal wastes space inside allocated blocks/pages; external leaves free space split into unusable fragments.
- **Paging vs segmentation: how do they differ?**
  - **Answer:** Paging divides memory into fixed-sized units; segmentation uses variable-sized logical regions such as code/data/stack.
- **What is a page-replacement algorithm? Compare FIFO, LRU and Optimal.**
  - **Answer:** FIFO evicts the oldest loaded page; LRU evicts least recently used; Optimal evicts the page whose next use is farthest away (theoretical benchmark).
- **What is Belady's anomaly?**
  - **Answer:** With FIFO, adding page frames can sometimes increase page faults; stack algorithms like LRU do not exhibit this anomaly.
- **What is thrashing?**
  - **Answer:** Excessive paging consumes resources because processes' working sets do not fit effectively in available memory.
- **What is the working-set model?**
  - **Answer:** Tracks pages used in a recent window to estimate a process's actively needed memory.
- **What is copy-on-write (COW)?**
  - **Answer:** Share read-only memory pages until a writer modifies one, then create a private copy; commonly used after `fork()`.
- **What are stack overflow and segmentation fault?**
  - **Answer:** Stack overflow exhausts the stack (often from deep recursion); a segmentation fault typically signals invalid/prohibited memory access.

### E. IPC, files and I/O

- **What is IPC (Inter-Process Communication)? Name common methods.**
  - **Answer:** Mechanisms for cooperating processes: pipes, message queues, shared memory, sockets and signals.
- **Shared memory vs message passing: trade-offs?**
  - **Answer:** Shared memory can avoid copying but needs synchronization; message passing provides an explicit communication boundary.
- **What is a pipe? How is a named pipe different?**
  - **Answer:** A pipe is a byte stream between endpoints; a named pipe has a filesystem-visible name and can connect unrelated processes.
- **What is a file descriptor?**
  - **Answer:** A process-local integer handle referring to an open file/socket/pipe or other kernel-managed resource.
- **What is an inode?**
  - **Answer:** In Unix-like filesystems, metadata describing a file such as permissions and data-block pointers, separate from directory filenames.
- **Hard link vs symbolic link?**
  - **Answer:** Hard links are directory entries for the same inode; symbolic links store a path and may point to missing targets.
- **What is buffering vs caching?**
  - **Answer:** Buffering smooths data transfer between producers/consumers; caching retains reusable data to avoid repeated expensive access.
- **What are synchronous, asynchronous, blocking and non-blocking I/O?**
  - **Answer:** Blocking says whether a call waits before returning; synchronous/asynchronous describes how I/O completion is coordinated. These are related but not identical axes.
- **What is a memory-mapped file?**
  - **Answer:** A file whose bytes are mapped into a process address space so code can access them through ordinary memory operations.
- **What is the difference between a system call and an interrupt?**
  - **Answer:** A system call is a deliberate software request for kernel service; a hardware interrupt asynchronously signals an external event (terminology varies by CPU).

---

## 4. Computer Networks (CN)

### A. Layers and network basics

- **What is a computer network? What are latency, bandwidth and throughput?**
  - **Answer:** A network connects devices to exchange data. Latency is delay, bandwidth is potential capacity, and throughput is achieved data rate.
- **Explain the seven OSI layers.**
  - **Answer:** Physical, Data Link, Network, Transport, Session, Presentation, Application. It is a reference model, not a literal mapping for every internet protocol.
- **Explain the TCP/IP model and its mapping to OSI.**
  - **Answer:** Commonly Link, Internet, Transport, Application; OSI Session/Presentation functions are often handled within applications/libraries.
- **What is encapsulation and decapsulation?**
  - **Answer:** Sending layers wrap payloads with headers (and sometimes trailers); receiving layers remove them to deliver higher-layer data.
- **What is the difference between a hub, switch and router?**
  - **Answer:** Hub repeats bits, switch forwards link-layer frames based on MAC addresses, router forwards packets across IP networks.
- **What are MAC and IP addresses?**
  - **Answer:** MAC identifies link-layer interfaces on a local network; IP identifies network-layer endpoints for routing.
- **What is a packet vs a frame vs a segment?**
  - **Answer:** Frame is link-layer unit; packet is typically IP network-layer unit; TCP segment is transport-layer unit.
- **What is unicast vs multicast vs broadcast?**
  - **Answer:** One receiver, selected receiver group, or all nodes on a local broadcast domain, respectively.
- **What is a socket? What is a port?**
  - **Answer:** Socket is an OS communication endpoint; a port identifies a transport-layer service/process endpoint within an IP host.
- **What are connection-oriented and connectionless communication?**
  - **Answer:** Connection-oriented protocols establish shared connection state; connectionless protocols send independent datagrams.
- **What is packet switching vs circuit switching?**
  - **Answer:** Packet switching shares network capacity among independently routed packets; circuit switching reserves a path/resources for a session.

### B. IP, subnetting, local networks and DNS

- **What is IPv4 vs IPv6?**
  - **Answer:** IPv4 uses 32-bit addresses; IPv6 uses 128-bit addresses plus revised headers and address-configuration mechanisms.
- **What is a subnet mask / CIDR notation?**
  - **Answer:** `/n` indicates the number of leading network bits, e.g. `192.168.1.0/24` has a 24-bit network prefix.
- **How many addresses are in an IPv4 `/26` subnet?**
  - **Answer:** `2^(32−26) = 64` total addresses, typically 62 usable host addresses in a conventional subnet.
- **What is a default gateway?**
  - **Answer:** A router to which a host sends packets for destinations outside its local network.
- **What is ARP? When is it used?**
  - **Answer:** Address Resolution Protocol resolves an IPv4 address to a link-layer address on the local network.
- **What is DHCP?**
  - **Answer:** Dynamic Host Configuration Protocol supplies IP settings such as address, subnet mask, gateway and DNS servers.
- **What is DNS? Explain the resolution flow.**
  - **Answer:** DNS maps domain names to records. A resolver checks cache, then queries DNS hierarchy (root/TLD/authoritative), usually through a recursive resolver.
- **What is the difference between DNS A, AAAA, CNAME and MX records?**
  - **Answer:** A maps to IPv4, AAAA to IPv6, CNAME aliases a domain name, and MX selects mail exchangers.
- **What is DNS TTL?**
  - **Answer:** Time-to-live sets how long a resolver may cache a DNS record before revalidation.
- **What is NAT/PAT? Why are they used?**
  - **Answer:** NAT translates IP addresses; PAT also maps ports, commonly allowing many private devices to share a public IPv4 address.
- **What is the difference between private and public IP addresses?**
  - **Answer:** Public addresses are globally routable; private IPv4 ranges are intended for internal networks and usually require NAT to reach the public internet.
- **What is ICMP? How do `ping` and `traceroute` use it?**
  - **Answer:** ICMP carries control/errors. Ping commonly uses Echo messages; traceroute probes paths using TTL/hop-limit expiry and received responses.
- **What is routing vs switching?**
  - **Answer:** Routing chooses network-layer next hops across networks; switching forwards link-layer frames within a LAN.
- **What is the difference between distance-vector and link-state routing? [Stretch]**
  - **Answer:** Distance-vector exchanges route estimates with neighbors; link-state floods topology information and computes shortest paths locally.

### C. TCP, UDP, reliability and performance

- **TCP vs UDP: when would you use each?**
  - **Answer:** TCP provides a reliable ordered byte stream; UDP sends datagrams without built-in delivery/order guarantees. Choose based on application's needs.
- **Explain the TCP three-way handshake.**
  - **Answer:** SYN → SYN-ACK → ACK, establishing connection state and initial sequence-number agreement.
- **Why does TCP need a three-way handshake?**
  - **Answer:** Both sides confirm reachability and synchronize initial sequence numbers while reducing confusion from stale connection attempts.
- **Explain TCP connection termination.**
  - **Answer:** Typically each direction closes independently with FIN and ACK exchanges; a reset (RST) can abort a connection.
- **What are TCP sequence and acknowledgment numbers?**
  - **Answer:** Sequence numbers locate bytes in the stream; ACKs normally indicate the next byte expected in-order.
- **How does TCP retransmission work?**
  - **Answer:** A sender retransmits suspected lost data based on timers and duplicate/selective acknowledgment signals.
- **What is a TCP sliding window?**
  - **Answer:** It allows multiple unacknowledged bytes in flight, improving utilization over stop-and-wait transmission.
- **Flow control vs congestion control: what differs?**
  - **Answer:** Flow control protects the receiver (advertised receive window); congestion control protects the network from overloaded paths (congestion window).
- **What are TCP slow start and congestion avoidance?**
  - **Answer:** Congestion window initially grows quickly, then more cautiously after a threshold, reacting to losses/congestion signals (exact behavior depends on algorithm).
- **What is RTT and why does it matter?**
  - **Answer:** Round-Trip Time is time for a request/ack path; it affects TCP throughput, retransmission timers and interactive latency.
- **What is the bandwidth-delay product?**
  - **Answer:** `bandwidth × RTT` estimates bytes that can be in flight to fully utilize a path.
- **What is packet loss, jitter and out-of-order delivery?**
  - **Answer:** Loss drops packets; jitter is variation in delay; out-of-order delivery changes arrival sequence. Their impact depends on transport/application.
- **Why can UDP still be suitable for video calls or games?**
  - **Answer:** Apps may prefer timely updates over retransmitting obsolete data, and can implement specialized recovery/jitter handling.
- **What is TCP head-of-line blocking?**
  - **Answer:** A missing earlier byte prevents later received bytes from being delivered in order to the application on that TCP stream.
- **What is a TCP connection identified by?**
  - **Answer:** Typically the tuple (source IP, source port, destination IP, destination port), together with protocol identity.

### D. HTTP, web application networking and security

- **What happens when you type a URL in your browser and press Enter?**
  - **Answer:** Browser checks cache, resolves DNS, establishes transport/security connection, sends HTTP request, receives response and parses/renders resources.
- **What is HTTP? Is it stateful?**
  - **Answer:** HTTP is an application-layer request/response protocol and is stateless in its semantics; sessions are layered using cookies/tokens.
- **Explain GET, POST, PUT, PATCH and DELETE.**
  - **Answer:** GET retrieves; POST submits/processes; PUT replaces target representation; PATCH partially changes; DELETE removes target resource. Actual app behavior must respect its API contract.
- **What is idempotency in HTTP? Which methods are intended to be idempotent?**
  - **Answer:** Repeating an idempotent request has the same intended server effect as sending it once. GET, PUT and DELETE are idempotent by definition; POST generally isn't.
- **What are common HTTP status codes?**
  - **Answer:** 200 OK, 201 Created, 204 No Content, 301/302 redirects, 400 Bad Request, 401 Unauthorized (authentication needed), 403 Forbidden, 404 Not Found, 409 Conflict, 429 Too Many Requests, 500 Server Error, 503 Unavailable.
- **HTTP vs HTTPS?**
  - **Answer:** HTTPS is HTTP over TLS, providing encryption, integrity and authenticated server identity when certificate verification succeeds.
- **What is TLS and what does a TLS handshake establish?**
  - **Answer:** TLS negotiates cryptographic parameters and keys and authenticates the server (optionally client) before protected communication.
- **What is an SSL/TLS certificate?**
  - **Answer:** A signed credential binding a public key to an identity such as a domain, allowing clients to authenticate the server.
- **What is the difference between HTTP/1.1, HTTP/2 and HTTP/3?**
  - **Answer:** HTTP/1.1 uses text framing and commonly multiple TCP connections; HTTP/2 multiplexes binary streams over TCP; HTTP/3 uses QUIC over UDP with independent streams.
- **What is a cookie vs a server-side session?**
  - **Answer:** Cookies store small client-side values sent with requests; a server-side session stores application state keyed by a session identifier.
- **What is JWT? How is it different from a session ID?**
  - **Answer:** A JWT is a signed token carrying claims (typically readable, not encrypted); a session ID is usually an opaque key to server-stored session state.
- **What is CORS? Is it a server-to-server security boundary?**
  - **Answer:** Cross-Origin Resource Sharing is a browser-enforced permission mechanism for cross-origin reads; non-browser clients are not restricted by it.
- **What is the same-origin policy?**
  - **Answer:** Browsers restrict how one origin's scripts interact with resources from another origin, with controlled exceptions such as CORS.
- **What is a reverse proxy? How is it different from a forward proxy?**
  - **Answer:** Forward proxy represents clients; reverse proxy represents servers and can terminate TLS, route requests or load-balance.
- **What is a load balancer? L4 vs L7?**
  - **Answer:** Distributes requests across backends. L4 works on transport-level connections; L7 can use HTTP paths, hosts, headers and other application data.
- **What is a CDN?**
  - **Answer:** A geographically distributed network caching/serving content closer to users to reduce latency and origin load.
- **What is WebSocket? When would you use it?**
  - **Answer:** A persistent full-duplex application connection, commonly established using an HTTP upgrade, suitable for chats and real-time dashboards.
- **What is REST?**
  - **Answer:** An architectural style for resource-oriented stateless interactions; well-designed REST APIs use consistent resource identifiers and standard HTTP semantics.
- **What is a firewall?**
  - **Answer:** Software/hardware enforcing traffic rules based on factors such as IP, port, protocol, connection state and sometimes application content.

---

## 5. Structured Query Language (SQL)

> **SQL is intentionally separate from DBMS.** DBMS asks how data is modeled, stored and coordinated; SQL asks how to retrieve and modify it. SQL dialect differences matter (PostgreSQL, MySQL, SQL Server, etc.). The solutions below use **PostgreSQL-style SQL**.

### A. SQL language and basic querying

- **What is SQL? What are DDL, DML, DCL and TCL?**
  - **Answer:** SQL is a language for relational databases. DDL defines structures (`CREATE`, `ALTER`), DML changes/queries rows (`INSERT`, `UPDATE`, `DELETE`, `SELECT`), DCL manages permissions, and TCL controls transactions.
- **What is the difference between `WHERE` and `HAVING`?**
  - **Answer:** `WHERE` filters input rows before grouping; `HAVING` filters groups after aggregate calculation.
- **What is the logical processing order of a SELECT query?**
  - **Answer:** Conceptually `FROM/JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT/OFFSET` (details vary with features like windows).
- **What does `GROUP BY` do?**
  - **Answer:** Groups rows by key so aggregate functions can calculate one result per group.
- **What is `DISTINCT`? How does it differ from `GROUP BY`?**
  - **Answer:** DISTINCT removes duplicate result rows; GROUP BY forms groups and commonly supports aggregates.
- **What does `ORDER BY` do? What happens if it is omitted?**
  - **Answer:** Sorts results; without it, row order is not guaranteed even when the output seems stable.
- **What is `LIMIT/OFFSET`? Why can large offsets be slow?**
  - **Answer:** Limits and skips rows; large OFFSET values can force processing many discarded rows. Keyset pagination may be faster.
- **What is `NULL`? Why is `= NULL` incorrect?**
  - **Answer:** NULL means unknown/missing, not a normal value. Use `IS NULL`; SQL uses three-valued logic (TRUE/FALSE/UNKNOWN).
- **How do `COUNT(*)`, `COUNT(col)` and `COUNT(DISTINCT col)` differ?**
  - **Answer:** Count all rows, count non-NULL values and count distinct non-NULL values, respectively.
- **What are `COALESCE`, `NULLIF` and `CASE` used for?**
  - **Answer:** COALESCE chooses first non-NULL value; NULLIF returns NULL if two expressions match; CASE creates conditional expressions.
- **What is the difference between `LIKE` and `ILIKE`?**
  - **Answer:** LIKE matches patterns; PostgreSQL's ILIKE is case-insensitive. `%` means any-length sequence, `_` one character.
- **What is the difference between `IN`, `BETWEEN` and `EXISTS`?**
  - **Answer:** IN tests membership, BETWEEN checks an inclusive range, EXISTS checks whether a subquery returns any row.
- **What are `INSERT`, `UPDATE`, `DELETE` and `TRUNCATE`?**
  - **Answer:** INSERT adds rows; UPDATE changes rows; DELETE removes selected rows; TRUNCATE quickly removes all rows and has DB-specific logging/transaction semantics.
- **What is an UPSERT?**
  - **Answer:** An insert that updates or does nothing when a unique-key conflict occurs, e.g. PostgreSQL `INSERT ... ON CONFLICT ... DO UPDATE`.
- **What is `RETURNING` in PostgreSQL?**
  - **Answer:** Returns rows affected by INSERT/UPDATE/DELETE, useful for getting generated IDs or updated values without another query.

### B. Joins, subqueries and set operations

- **What is `INNER JOIN`?**
  - **Answer:** Returns rows from both tables where the join condition matches.
- **What is `LEFT JOIN`?**
  - **Answer:** Keeps every row from the left table and fills unmatched right-side columns with NULL.
- **How do RIGHT JOIN and FULL OUTER JOIN differ?**
  - **Answer:** RIGHT preserves all right-side rows; FULL preserves unmatched rows from both sides.
- **What is CROSS JOIN?**
  - **Answer:** Returns the Cartesian product: every left row paired with every right row.
- **What is a SELF JOIN?**
  - **Answer:** Joining a table to itself, e.g. employees to managers via `manager_id`.
- **What is the difference between filtering in `ON` and `WHERE` for a LEFT JOIN?**
  - **Answer:** An `ON` predicate controls matched right rows while preserving left rows; filtering right-side columns in `WHERE` can remove unmatched NULL rows and behave like an INNER JOIN.
- **Why do joins sometimes create duplicate rows?**
  - **Answer:** A one-to-many relationship yields one output row per match; verify join keys and cardinality before blindly adding DISTINCT.
- **What is a subquery? What is a correlated subquery?**
  - **Answer:** A subquery is nested SQL; a correlated subquery refers to columns from an outer query and is logically evaluated per outer row (optimizer may rewrite it).
- **What is a CTE (`WITH` clause)?**
  - **Answer:** A named subquery that makes complex queries more readable; it isn't always materialized, depending on the DBMS and query.
- **What is the difference between `UNION` and `UNION ALL`?**
  - **Answer:** UNION removes duplicate rows across inputs; UNION ALL preserves duplicates and avoids deduplication work.
- **What do `INTERSECT` and `EXCEPT` do?**
  - **Answer:** INTERSECT returns common rows; EXCEPT returns rows from the first input absent in the second (default set semantics remove duplicates).
- **What are `EXISTS` and `NOT EXISTS` useful for?**
  - **Answer:** Checking that matching rows do or do not exist, including anti-joins without NULL complications from `NOT IN`.
- **Why can `NOT IN (subquery)` behave unexpectedly with NULL?**
  - **Answer:** If the subquery contains NULL, comparisons may become UNKNOWN and filter out rows you expected; `NOT EXISTS` is usually safer.

### C. Aggregates, windows, constraints and performance

- **What is a window function? How is it different from GROUP BY?**
  - **Answer:** It computes over related rows without collapsing them into one row per group, using `OVER(PARTITION BY ... ORDER BY ...)`.
- **`ROW_NUMBER()` vs `RANK()` vs `DENSE_RANK()`?**
  - **Answer:** ROW_NUMBER always gives unique sequence positions; RANK leaves gaps after ties; DENSE_RANK doesn't leave gaps.
- **What are `LAG()` and `LEAD()`?**
  - **Answer:** Access values from prior/next rows in the specified window order, useful for deltas and comparisons.
- **What is a running total?**
  - **Answer:** A cumulative sum computed over ordered rows, e.g. `SUM(amount) OVER(ORDER BY date, id ROWS UNBOUNDED PRECEDING)`.
- **What is a moving average?**
  - **Answer:** Average over a sliding window, e.g. current row and two previous rows using `ROWS BETWEEN 2 PRECEDING AND CURRENT ROW`.
- **What is the difference between `ROWS` and `RANGE` window frames?**
  - **Answer:** ROWS counts physical rows; RANGE groups rows by ordering values and a value-based frame. Ties can behave differently.
- **How do `PRIMARY KEY` and `UNIQUE` affect INSERTs?**
  - **Answer:** They reject conflicting keys; a primary key additionally cannot be NULL.
- **What is a SQL transaction block?**
  - **Answer:** Statements between `BEGIN` and `COMMIT`/`ROLLBACK` treated as one transaction.
- **What is SQL injection? How do prepared statements prevent it?**
  - **Answer:** Untrusted input changes SQL structure when interpolated into query strings. Parameter binding sends values separately from SQL code.
- **How can you improve a slow SQL query?**
  - **Answer:** Inspect `EXPLAIN ANALYZE`, filter early, avoid excessive fetched columns, add appropriate indexes, reduce N+1 patterns and reconsider joins/grouping.
- **What is an index scan vs sequential scan in PostgreSQL?**
  - **Answer:** A sequential scan reads table pages; an index scan uses an index to find candidate tuples, sometimes with extra heap lookups.
- **What is `SELECT ... FOR UPDATE`?**
  - **Answer:** Locks selected rows for a transaction so concurrent updates/deletes must coordinate, useful for read-modify-write logic.

### D. SQL query challenges — try before reading the solutions

**Practice schema (PostgreSQL):**

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name TEXT NOT NULL,
    department_id INT,
    salary INT,
    manager_id INT
);

CREATE TABLE departments (
    id INT PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE customers (
    id INT PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE orders (
    id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    amount NUMERIC(12, 2),
    status TEXT
);

CREATE TABLE logins (
    user_id INT NOT NULL,
    login_date DATE NOT NULL
);
```

#### 1. Find employees earning more than 80,000.

```sql
SELECT id, name, salary
FROM employees
WHERE salary > 80000;
```

#### 2. Find the second-highest **distinct** salary (return NULL if none).

```sql
SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

#### 3. Find the third-highest **distinct** salary.

```sql
SELECT salary
FROM (
    SELECT DISTINCT salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM employees
    WHERE salary IS NOT NULL
) t
WHERE rnk = 3;
```

#### 4. Find the highest-paid employee(s) in each department, including ties.

```sql
SELECT id, name, department_id, salary
FROM (
    SELECT e.*,
           DENSE_RANK() OVER (
               PARTITION BY department_id ORDER BY salary DESC
           ) AS rnk
    FROM employees e
    WHERE salary IS NOT NULL
) t
WHERE rnk = 1;
```

#### 5. Find departments with more than three employees.

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 3;
```

#### 6. Show each employee's name with their manager's name.

```sql
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;
```

#### 7. Find employees who earn more than their manager.

```sql
SELECT e.id, e.name
FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

#### 8. Find departments with no employees.

```sql
SELECT d.id, d.name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1 FROM employees e WHERE e.department_id = d.id
);
```

#### 9. Find duplicate employee names and their counts.

```sql
SELECT name, COUNT(*) AS cnt
FROM employees
GROUP BY name
HAVING COUNT(*) > 1;
```

#### 10. Find customers who have never placed an order.

```sql
SELECT c.id, c.name
FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM orders o WHERE o.customer_id = c.id
);
```

#### 11. Calculate total completed order revenue for every customer, including zero-revenue customers.

```sql
SELECT c.id, c.name,
       COALESCE(SUM(o.amount), 0) AS revenue
FROM customers c
LEFT JOIN orders o
  ON o.customer_id = c.id AND o.status = 'completed'
GROUP BY c.id, c.name;
```

#### 12. Find the five customers with the highest completed order revenue.

```sql
SELECT c.id, c.name, SUM(o.amount) AS revenue
FROM customers c
JOIN orders o ON o.customer_id = c.id
WHERE o.status = 'completed'
GROUP BY c.id, c.name
ORDER BY revenue DESC, c.id
LIMIT 5;
```

#### 13. Compute the running total of completed order amounts by date and order ID.

```sql
SELECT id, order_date, amount,
       SUM(amount) OVER (
           ORDER BY order_date, id
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) AS running_total
FROM orders
WHERE status = 'completed';
```

#### 14. Find each customer's most recent order (including ties broken by highest ID).

```sql
SELECT id, customer_id, order_date, amount
FROM (
    SELECT o.*,
           ROW_NUMBER() OVER (
               PARTITION BY customer_id
               ORDER BY order_date DESC NULLS LAST, id DESC
           ) AS rn
    FROM orders o
) t
WHERE rn = 1;
```

#### 15. Find the top two distinct salaries in every department (include ties).

```sql
SELECT id, name, department_id, salary
FROM (
    SELECT e.*,
           DENSE_RANK() OVER (
               PARTITION BY department_id ORDER BY salary DESC
           ) AS rnk
    FROM employees e
    WHERE salary IS NOT NULL
) t
WHERE rnk <= 2;
```

#### 16. Find the difference between each employee's salary and their department average.

```sql
SELECT id, name, department_id, salary,
       salary - AVG(salary) OVER (
           PARTITION BY department_id
       ) AS difference_from_avg
FROM employees;
```

#### 17. Count distinct active customers by month (customers with at least one order).

```sql
SELECT DATE_TRUNC('month', order_date)::date AS month,
       COUNT(DISTINCT customer_id) AS active_customers
FROM orders
WHERE order_date IS NOT NULL
GROUP BY 1
ORDER BY 1;
```

#### 18. Find daily order revenue and its change from the previous day with orders.

```sql
WITH daily AS (
    SELECT order_date, SUM(amount) AS revenue
    FROM orders
    WHERE status = 'completed'
    GROUP BY order_date
)
SELECT order_date, revenue,
       revenue - LAG(revenue) OVER (ORDER BY order_date)
         AS change_vs_previous_order_day
FROM daily
ORDER BY order_date;
```

#### 19. Find users who logged in on **three consecutive calendar days**.

```sql
WITH days AS (
    SELECT DISTINCT user_id, login_date
    FROM logins
), numbered AS (
    SELECT user_id, login_date,
           ROW_NUMBER() OVER (
               PARTITION BY user_id ORDER BY login_date
           ) AS rn
    FROM days
), grouped AS (
    SELECT user_id, login_date - rn::int AS grp
    FROM numbered
)
SELECT DISTINCT user_id
FROM grouped
GROUP BY user_id, grp
HAVING COUNT(*) >= 3;
```

#### 20. Delete duplicate `(user_id, login_date)` records, keeping one row per pair (PostgreSQL).

```sql
DELETE FROM logins a
USING logins b
WHERE a.ctid > b.ctid
  AND a.user_id IS NOT DISTINCT FROM b.user_id
  AND a.login_date IS NOT DISTINCT FROM b.login_date;
```

**Note:** Challenge 20 uses PostgreSQL's physical row locator `ctid` for a one-time cleanup. In a real application prefer a stable unique row ID and enforce `UNIQUE(user_id, login_date)` to prevent duplicates. For challenge 19, subtracting `ROW_NUMBER()` from each distinct date identifies consecutive-day islands.

---

## 6. High-Yield Scenario Questions

These questions combine multiple concepts and are frequently useful in project-defense or backend discussions.

- **Two users book the same last seat concurrently. How do you prevent double booking?**
  - **Answer:** Enforce a unique seat/booking constraint and use a transaction with an atomic conditional claim (or appropriate row locks); handle conflicts/retries.
- **Your query works locally but is slow on a table with ten million rows. What do you check first?**
  - **Answer:** Query plan and actual row estimates, indexes on filtering/joining columns, cardinalities, unnecessary rows/columns, N+1 behavior and sorting.
- **Two threads increment a shared integer one million times. Why may the result be wrong?**
  - **Answer:** Read-modify-write is not atomic; increments can be lost. Use an atomic counter or a mutex.
- **Your service crashes while holding a mutex. What happens?**
  - **Answer:** An ordinary process-local mutex and its process memory disappear when the process exits; other processes should not rely on it for cross-process recovery. Robust/shared mutexes need explicit owner-death handling.
- **Why might a page fault occur even when your machine has free RAM?**
  - **Answer:** A valid page may not yet be mapped/populated (demand paging or copy-on-write); a page fault does not necessarily imply RAM exhaustion.
- **A website loads by IP but not by domain name. What is a likely issue?**
  - **Answer:** Investigate DNS resolution, records, caches and resolver configuration (then check TLS Host/SNI and virtual hosting, since IP-only access may differ).
- **Why does a chat app use WebSockets but a regular CRUD API use HTTP?**
  - **Answer:** WebSockets efficiently support persistent bidirectional events; standard HTTP is simple for occasional stateless request/response operations.
- **A user retries POST after a timeout and creates two payments. How do you fix this?**
  - **Answer:** Use idempotency keys stored atomically with request outcomes, along with unique constraints and correctly scoped transactions.
- **A LEFT JOIN query unexpectedly omits customers with no orders. What is likely wrong?**
  - **Answer:** A condition on right-table columns was placed in WHERE; move relevant filters into ON or explicitly include NULLs.
- **You delete a derived object through a `Base*` and the derived destructor doesn't run. Why?**
  - **Answer:** The base destructor was not virtual; deleting through such a base pointer has undefined behavior in ordinary polymorphic deletion.
- **Your process is responsive with one thread but slow with 100 threads. Why?**
  - **Answer:** Investigate contention, excessive context switches, memory/cache pressure, CPU oversubscription and blocking I/O.
- **A cache has stale data after an update. How do you reason about the fix?**
  - **Answer:** Identify the source of truth and write order; invalidate/update relevant keys after a successful commit or adopt explicit freshness/TTL guarantees.

---

## 7. Final Revision Checklist

| Subject | Must be able to explain without notes | Must be able to solve / demonstrate |
|---|---|---|
| **DBMS** | Keys, normalization, ACID, isolation anomalies, indexing, joins, MVCC, locks | Design 3–4 related tables; reason about a lost-update/double-booking race; read a basic query plan |
| **OOP (C++)** | Four pillars, overloading vs overriding, virtual dispatch, destructor, copy vs move, RAII, smart pointers | Write a small abstract base + derived implementation; show safe ownership and copying |
| **OS** | Process vs thread, scheduling, sync primitives, deadlock, paging, TLB, page faults, IPC | Compute a scheduling timeline; identify race/deadlock; walk through a page-fault scenario |
| **CN** | OSI/TCP-IP, DNS/ARP/DHCP/NAT, TCP handshake, flow vs congestion control, HTTP/HTTPS/TLS | Explain entering a URL; troubleshoot DNS vs TCP vs HTTP failure; choose TCP/UDP/WebSocket |
| **SQL** | Joins, WHERE/HAVING, NULL, GROUP BY, subqueries, CTE, window functions | Write joins, second-highest salary, top-N per group, anti-joins, cumulative sums and consecutive-day queries |

**Placement-focused rule:** Breadth matters, but for any question you should be able to defend at least *one concrete example* and *one trade-off*. The bank is a study guide, not a claim that no interviewer will ask beyond it.

## Further Reading

- **DBMS and SQL:** [PostgreSQL Documentation](https://www.postgresql.org/docs/current/), [SQLBolt interactive lessons](https://sqlbolt.com/)
- **C++ / OOP:** [cppreference](https://en.cppreference.com/w/)
- **Operating Systems:** [Operating Systems: Three Easy Pieces](https://pages.cs.wisc.edu/~remzi/OSTEP/)
- **Computer Networks / Web:** [MDN HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview), [Cloudflare Learning Center](https://www.cloudflare.com/learning/)

---

*Original, curated CS interview-preparation companion inspired by the sectioned question-and-answer organization of the user-provided ML/DL README. It is not a reproduction of that repository's content.*
