# Architecture of a DigMem system

Read [philosophy.md](../philosophy.md) first.

## The name

"Dig" has two meanings:

- Memory is organized as digestions
- Progressive disclosure is digging the history

## History

The history consists of multiple immutable commits.

To capture what happened, each commit contains a task note describing information, including but not limited to:

- background: why to do this task
- goal: what to do in this task
- constraint: what not to do in this task
- boundary: things not cared in this task
- fact: things derived from project state
- reference: things picked from previous commits, web or memory
- question: what remains uncertain
- assumption: additional conditions to help answer questions
- decision: answers to questions
- progress: what happened in the task execution
- unhappiness: what goes wrong in the mid of task execution
- signal: does previous memory help or hurt?
- result: things done in this task
- future work: things not done in this task, but deserved to do

## Index

### Structure

The index is organized a digestion tree.
Each node of the tree contains a digestion with length limit, created by agents.

#### Statement

Information in digestion tree is formulated to an standard immutable format: statement, for easy tracing and reference.

Each statement is a few sentences with some metadata:

- a multi-word label like `short-description`, unique in the same node
- a single-word type like `fact` or `assumption`

Then statements can be globally indexed by unique ID `[node ID]_[type]_[short-description]`.

#### Leaf node

The digestion tree will create an immutable leaf node for each commit, containing statements directly derived from the task note.

#### Non-leaf node

Non-leaf nodes also contain statements, including:

- existing one picked from a children node, whose original statement ID would be reused.
- new one generated in the digestion, with its own label and type

The non-leaf node becomes immutable once its degree is full and all children are immutable.
Before that, non-leaf nodes are versioned.

### Evolvement

The memory evolvement happens on non-leaf node update, guided by digestion instruction, including:

#### Cluster and enhancement

Rationale: statements appearing a lot are important.

The digestion can deduplicate similar statements, but enhance them by summing the weight.

#### Signal passing

Rationale: signals from new commits direct selection of old statements.

If there are positive/negative signals for an old statement, that statement should be promoted/demoted accordingly.

Signals should also be clustered and passed to higher layer, to evaluate older statements.
Note that:

- conflict signals should be summed, not corrected.
- signals in the oldest children of root node can be dropped safely, because there are no older statements.

## Collaboration

Modern version-control systems allow multiple parallel feature development.
However, time-based signal passing requires a linear order.

The principle is respecting the casual order happened in development, and treating each branch interaction is a one-off update on current branch.

## Tool

DigMem provides a command line tool `digmem` to manage memory data and maintain integrity.

### Submit commit

`digmem check-note FILE` will check whether the file contains required task note.

### Update the tree

`digmem check-tree` will report whether there are new digestions to generate.

Agents can submit digestion update by `digmem update-tree [node-ID] FILE`.

### Read memory

`digmem show [node-ID]` can read the content of given node.
The root node will be read if no argument is given.

`digmem show statement-ID` can read the content of given node.

## Performance

### object packing

Digestion tree node length is limited, so there are many small objects.
To reduce listing cost, once a subtree is sealed, the whole tree can be packed into a large file with offset index.

## Configuration

Following things can be configured by users:

## Text

- commit task note template
- digestion instruction

## Tree

- digestion tree node length limit (default 4K bytes)
- max degree of digestion tree (default 2)
