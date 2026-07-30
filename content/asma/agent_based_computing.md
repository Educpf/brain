# What is an agent?

A general definition could be: An **agent** is a computing entity in an **environment**, capable of **autonomous** action to fulfill its designed objectives. It perceives the env via **sensors** and acts upon via **actuators** ( sees specific stuff and acts in specific ways )

But the best way to understand what an agent is would be to compare it directly to already known concepts and actually see how does it fit and relate with them. ( This is actually how humans learn, new concepts are better learned when related with previous ones )

## Intelligent 

Defined as the competence to execute "complex" tasks that require intelligence, rather than strictly matching human performance or perfect rational. An agent might need or not **intelligence** to master its tasks, depending on the environment. When comparing with **traditional AI**, which focuses on building individual components of intelligence ( learning, planning, understanding), the agent component is worried about integrating those to make independent decisions, focusing on **social abilities** ( communication, cooperation, negotiation etc...)

## Autonomous

The ability to perform tasks to achieve goals without recourse to the agent's designer. The **alignment problem** focuses on the fact that as systems become more autonomous it becomes crucial to properly specify its objectives. Systems use **adjustable autonomy** with **human in the loop** to ensure that when necessary.

## Complex System

No real clear definition, but can be understood as networks of many interacting, decentralized and self-organized components. MAS are equipped to model and simulate these systems by decomposing complex behaviors into individual roles ( each agent acts as one, with its specific persona)

## Multi Agent Systems (MAS)

In these systems agents have **incomplete information** or **capabilities** and there is **no global system control**, data is **decentralized** and computation is **asynchronous**. **Micro** design focuses on building the individual agents while **Macro** on the mechanisms and protocols used to govern and enable interactions between the agents.


# Trends 

The justification for the transition toward agent-based computing can be supported by some ongoing technology trends

- **Ubiquity**: The rise of cheap computing devices
- **Interconnection**: Distributed computing systems at a large scale
- **Intelligence**: The capability of system to handle increasingly complex and automate tasks
- **Delegation**: Passing control of safety-critical tasks over computer systems
- **Human-Orientation**: Design software using concepts similar to how humans perceive and act on the world

# Applications

- Patient Scheduling in Hospitals: A decentralized system where agents represent patients (with varying priorities/health statuses) and hospital resources (ECG, X-rays, staff). They dynamically negotiate to schedule medical tests efficiently.
- Taxi-Sharing Solutions: An agent-based simulation to manage shared-use taxis. Central office agents and nearby taxi agents negotiate to pick up multiple passengers, optimizing travel times, customer experience, and cost-splitting criteria.
- Stock Market Trading (Board Game Simulation): Modeled after the game Panic on Wall Street, trading agents represent managers (trying to sell volatile or safe investment funds at high prices) and investors (trying to buy them at low prices), observing how stock values fluctuate based on negotiation rounds. 


