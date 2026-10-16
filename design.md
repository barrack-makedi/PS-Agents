Based on the class discussion on the self-driving trailer agent:
1.	Design the agent using the PEAS framework and further define its environment using the 5 definitions of agent environments.
An agent is anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators. 
The PEAS framework include:
•	Performance measures = Safety of passengers, cargo and other road users, Efficient consumption of fuel, Measure of travel time, Smooth navigation on the road, Maintenance of the vehicle and cargo and Accident aversion.
•	Environment = Roads, Road signs, Vehicles, Pedestrians, Traffic lights, Drivers, Traffic police checkpoints, Day or night conditions and Weather and Road conditions.
•	Actuators = Steering, Brakes, Indicators, Wiper detection, Automated transmission, Horn, Accelerator and Headlights
•	Sensors = Camera, GPS, Speedometer, Fuel-gauge, Mileage, Tyre pressure sensors, Clock and Radar. 

The agent environments
	Fully observable & partially observable
Fully Observable Agent: Information about the environment is fully available to the agent through its sensors.
Partially Observable Agent: Information about the environment is not fully available to the agent.
Thus, for the self-driving trailer agent it would most likely occupy the agent environment of partially observable agent, because the complete information regarding the environment is not available such as the radius coverage of the headlight.  
	Deterministic vs Stochastic
Deterministic Agent: Change in state of environment depends only on current state and agent’s action
Stochastic Agent: Environment has variety of aspects beyond the control of the agent.
Thus, for the self-driving trailer agent it would most likely occupy the agent environment of Stochastic agent, because certain variables are outside of the agents control such as weather:
	Episodic vs sequential
Episodic Agent: Choice of current action by agent is independent on previous actions.
Sequential Agent: Choice of action by agent is dependent on previous actions.
Thus, for the self-driving trailer agent it would most likely occupy the agent environment of Sequential agent, because every action taken by the trailer will affect any subsequent action made during the journey.
	Static vs dynamic
Static Agent: Environment does not change while the agent is deliberating to choose an action. 
Dynamic Agent: Environment can change while the agent is selecting an action – the agent would consult the environment first before acting.
Thus, for the self-driving trailer agent it would most likely occupy the agent environment of Dynamic agent, because changes in the environment are constantly changing such as traffic lights.
	Single agent vs multi-agent
Single agent: The agent is operating by itself in the environment. 
Multi-agent: The agent is operating with other agents in the environment.
Thus, for the self-driving trailer agent it would most likely occupy the agent environment of multi-agent, because the self-driving agent is in constant interaction with other agents in the road such as other vehicles and human agents like policemen which can affect its decisions and behaviour.
2.	develop a suitable directed graph and search tree based on the map presented in class of Kenya’s Highway Network featuring cities and towns in the Nairobi, Central Highlands, Rift Valley and Lake Victoria Basin regions. For reference, you can use this Google Maps view to generate the graph and tree. The tree should apply the concept of abstraction as discussed in class. 
Abstraction: Removes details that is not necessary to the route-level search, this makes the problem simple to solve.

Relevant detail:
•	Major towns
•	Main highway segments between neighbouring towns as actions
•	Road distance in km as the step cost
•	Direction of travel
Irrelevant detail:
•	Individual lanes, lane changes, speed and steering angle
•	Traffic lights, roundabouts, junctions and minor side roads
•	Small settlements and trading centres between nodes (e.g. Limuru, Mai Mahiu, Salgaa, Londiani are merged into the edges they lie on)
•	Traffic, weather, road works and driver behaviour
•	Time-of-day effects and fuel stops

Regions covered:
Region	Towns
Nairobi	Nairobi
Central Highlands	Thika, Muranga, Nyeri, Nyahururu
Rift Valley	Naivasha, Gilgil, Nakuru, Molo, Kericho, Eldoret
Lake Victoria Basin	Kakamega, Kisii, Kisumu
<img width="476" height="344" alt="image" src="https://github.com/user-attachments/assets/4181af89-806a-43db-870a-8ced62fd332a" />
<img width="715" height="238" alt="image" src="https://github.com/user-attachments/assets/4387ddc6-27cf-4541-9609-3e2d6a854c58" />
3.	using distance between towns as the path cost function, determine the optimal path the trailer can take from Nairobi to Kisumu. Calculate the distance between towns using this Google Maps search feature.
Uniform Cost Search is used because step costs differ (road distance). UCS always expands the frontier node with the lowest path cost g(n). It is complete (all step costs > 0) and optimal, since the goal is tested when a node is expanded, not when generated. Breadth-first search ignores distances and Depth-first and Greedy search do not guarantee the cheapest path.
Working:
Step	Expand(g)	Generated successors (g)	Frontier after expansion	Explored
0	Start	Nairobi (0)	Nairobi (0)	{}
1	Nairobi (0)	Thika (42), Naivasha (92)	Thika 42, Naivasha 92	Nairobi
2	Thika (42)	Murang’a (80)	Murang'a 80, Naivasha 92 	Nairobi, Thika
3	Murang’a (80)	Nyeri (137)	Naivasha 92, Nyeri 137	+Murang’a
4	Naivasha (92)	Gilgil (122)	Gilgil 122, Nyeri 137	+Naivasha
5	Gilgil (122)	Nakuru (156)	Nyeri 137, Nakuru 156	+Gilgil
6	Nyeri (137)	Nyahururu (222)	Nakuru 156, Nyahururu 222	+Nyeri
7	Nakuru (156)	Molo (205), Eldoret (311), Nyahururu (246, rejected: 222 is cheaper)	Molo 205, Nyahururu 222, Eldoret 311	+Nakuru
8	Molo (205)	Kericho (265)	Nyahururu 222, Kericho 265, Eldoret 311	+Molo
9	Nyahururu (222)	Nakuru (312, already explored at 156: discard)	Kericho 265, Eldoret 311	+Nyahururu
10	Kericho (265)	Kisumu (357), Kisii (370)	Eldoret 311, Kisumu 357, Kisii 370	+Kericho
11	Eldoret (311)	Kakamega (411)	Kisumu 357, Kisii 370, Kakamega 411	+Eldoret
12	Kisumu (357)	Goal test passed	n/a	STOP

Comparison of all possible routes.
Route	Towns	Total distance
A	Nairobi → Naivasha → Gilgil → Nakuru → Molo → Kericho → Kisumu	92 + 30 + 34 + 49 + 60 + 92 = 357
B	Nairobi → Naivasha → Gilgil → Nakuru → Eldoret → Kakamega → Kisumu	92 + 30 + 34 + 155 + 100 + 55 = 466
C	Nairobi → Naivasha → Gilgil → Nakuru → Molo → Kericho → Kisii → Kisumu	92 + 30 + 34 + 49 + 60 + 105 + 115 = 485
D	Nairobi → Thika → Murang'a → Nyeri → Nyahururu → Nakuru → Molo → Kericho → Kisumu	42 + 38 + 57 + 85 + 90 + 49 + 60 + 92 = 513

The final answer:<img width="556" height="52" alt="image" src="https://github.com/user-attachments/assets/e8db21fa-e261-474d-b912-924dda5eaeb1" />
Evidence:<img width="554" height="263" alt="image" src="https://github.com/user-attachments/assets/82fcc196-84bf-4bc5-bec6-3b74c2112888" />
