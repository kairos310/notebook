## Introduction to Software Engineering
differences between programming and software engineering
dimensions of software engineering (see lecture notes)
Hyrum’s Law
best practices:
	design software that is easy to change
		list some tings that may force software to change
	shift issue discovery “to the left”
### Version Control Systems
Why do we need VCS? (What problems does it solve?)
trunk-based development vs branch-based development
	understand the differences
	pros/cons of each
distributed vs. centralized VCS
best practices for branch-based development
### Git
What is Git?
basic Git architecture (see slides)
What is a local repository, and where is it stored in your local file system?
What is a remote repository? Where are these typically hosted?
What is a checksum, and how does Git use them?
basic terminology
untracked vs. tracked vs. modified vs. staged files
	staging area
	working tree
	local vs. remote
		origin
branches
	the main branch
	divergent branches
HEAD
	detached HEAD
commands
	git init vs. git clone
	git add
	git commit
	git checkout
	git fetch
	git push
	git pull
	git branch
	git merge
	git status
	git log
How does “git pull” differ from “git fetch”?
happy path workflow
	for trunk-based development
	for branch-based development
merging
	what is a fast-forward merge?
		When is a FF merge possible? (Be able to identify this on a diagram.)
be able to draw a diagram of the commit history after each command (see quiz #1)
	make sure you can do this for branch-based and trunk-based development
		see homework #1 be able to draw a diagram with basic components (see quiz #1)
What is GitHub?

## Software Development Life Cycle


List and describe the phases (see lecture notes)
Development methodologies (be able to describe and contrast them with each other)
	Waterfall
	Iterative Development
	Agile Development
		Agile Manifesto
			Individuals and interactions over processes and tools
			Working software over comprehensive documentation
			Customer collaboration over contract negotiation
			Responding to change over following a plan
*** No need to memorize this! Just understand the “Agile philosophy” and how
it differs from waterfall.

### Concept Phase
What are wireframes and story boards?
	be able to identify and draw these

### User Stories
[[User Stories]]
user story template (“As a…”)
general characteristics of good stories
writing user stories
evaluating user stories
	INVEST
splitting user stories (see Homework #2)
	techniques:
	Happy/Sad Path
	Workflow Steps
	Breadthwise Decomposition

### Design Diagrams
Understand and be able to explain/use the following types of diagrams:
	System Diagrams
	UML Sequence Diagrams
	Flow Charts
	UML Class Diagrams
Be able to describe the basic components of each type of diagram (see lecture notes)
When might you use each of these diagrams? (What types of things does it convey
about a system’s design?)
##### Architectural Design Patterns
model view controller (MVC)
	be able to describe the components of this architecture
	be able to draw a system diagram for it
	be able to draw a sequence diagram for it

##### Design Patterns
What are software design patterns, and why do we need them? (see lecture notes)
List several design principles that are used in most design patterns (see lecture notes)
Examples of design patterns:
	strategy
	observer
	decorator
Be able to describe the purpose of these design patterns
	What problems are they trying to solve?
Be able to identify one of these design patterns from a UML class diagram