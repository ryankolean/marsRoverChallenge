Challenege Instructions:
1.	Develop an api that moves a rover around on a grid.
2.	You are given the initial starting point (x,y) of a rover and the direction (N,S,E,W) it is facing.
3.	The rover receives a character array of commands.
4.	Implement commands that move the rover forward/backward (f,b).
5.	Implement commands that turn the rover left/right (l,r).
6.	Implement wrapping from one edge of the grid to another. (planets are spheres after all)
7.	Implement obstacle detection before each move to a new square. If a given sequence of commands encounters an obstacle, the rover moves up to the last possible point and reports the obstacle.	

	
Approach:
	Recieved email of the challenge and reviewed the instructions.
	Upon assesment, I would guess that this challenge would likely take me at around 8-16 hours (1-2 days) for me to complete, using tdd.
	So I decided to view the aspects of the issue with a google search, "mars rover python". 
	The results of the search yielded several different implementations with varying levels of complexity and real world implementability of the challenge.
	So given this, I didn't want to just plagerize any of the implementations that I had now reviewed and could just extend to what I specifically needed.
	Instead, I came up with a framework (see below) for what I think needs to implemented so if given more time and energy, the app could be built out to completetion.
	
	
Process:
	After laying out the basic structure of rover, position, and movement controllers, I started to think more about how the grid movement would be interpreted and 
	how the math for the movements would work. The rover's position and directionality are crucial to this step. 
	The grid that the rover would be using for positional placement is just like a bird's eye map.
	eg. [-2,3	-1,3	0,3		1,3		2,3	
		 -2,2	-1,2	0,2		1,2		2,2
		 -2,1	-1,1	0,1		1,1		2,1
		 -2,0	-1,0	0,0		1,0		2,0
		 -2,-1	0,-1	0,-1	1,-1	2,-1
		 -2,-2	0,-2	0,-2	1,-2	2,-2
		 -2,-3	0,-3	0,-3	1,3		2,-3]
	So if our rover, codename "wall-e", starts at location (1,1) and is facing north and we wanted to move him to location (0,3), there are multiple implementations to take into account.
	One solution: wall-e turns left and moves forward to get to (0,1), then turns right and moves forward 2.
	The input might look something like:
		01N
		turnL
		move1
		turnR
		move3
	The first line is the initial location (0,1) and direction (N)
	The following lines indicate actions for wall-e to take
	
	Taking the movement logic above, I decided that direction was the key for determing movement and updating wall-e's position.
	So I created the Position controller and add all the changes to move location within a cartesian coordinate system where:
		N is positive Y
		S is negative Y
		E is positive X
		W is negative X
	After that it was a matter of lining up the different movement options for each of the directions in the Movement controller.
	
	The final components of the challenge #6 and #7 require checks:
		#6 requires a check if the rover is at the edge of the grid before every movement step. The cartesian coordinate system makes this easier to process
			because wall-e's position will become the opposite value of the current position for the x or y coordinate.
		#7 requires a check if there is an obstacle obstructing movement. This can be handled by having a check for obstacles before wall-e executes movement is triggered.
			In the case of an obstacle, you would have an interrupt to send a message back "Houston we have a problem" and then halt processing of the input commands.
	
	
Framework(sudo-pythonic-code):
	Rover controller:
		initialize (starting position and direction)
		move (control the rover's movements & report obstacles)
		
	Movement controllers: handle movement (f,b) & turning (l,r)
		north:
			forward:
				position.up()
			backward:
				position.down()
			left:
				direction = W
			right:
				direction = E
		south:
			forward:
				position.down()
			backward:
				position.up()
			left:
				direction = E
			right:
				direction = W
		east:
			forward:
				position.right()
			backward:
				position.left()
			left:
				direction = N
			right:
				direction = S
		west:
			forward:
				position.left()
			backward:
				position.right()
			left:
				direction = S
			right:
				direction = N
	
	Position controllers:
		up
			position.y += 1
		down
			position.y -= 1
		left
			position.x -= 1
		right
			position.x += 1
	
	
Time Spent: 
	total: 1.75 hours
		initial research: 20 minutes
		framework: 45 minutes
		documentation: 40 minutes