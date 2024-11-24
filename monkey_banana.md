The Monkey and Banana problem is a classic test of artificial intelligence where a monkey is in a room with a banana hanging from the ceiling, out of reach. The monkey needs to figure out how to get the banana by using objects in the room. Here’s a detailed description of the problem setup:

# Problem Setup
- **Room Elements:**
  - A monkey at a certain location in the room.
  - A banana suspended from the ceiling.
  - A box that the monkey can move and climb on.

## Initial Conditions
- **Monkey Location:** The monkey starts at a location 'A' in the room.
- **Box Location:** The box is initially at location 'B'.
- **Banana Location:** The banana is at location 'C', hanging from the ceiling.

## Goal
- The monkey wants to get the banana.

## Constraints
- The monkey can only grab the banana if it is at the same location as the banana and on top of the box.
- The monkey can move horizontally between locations on the floor.
- The monkey can push the box from one location to another.
- The monkey can climb on top of the box.

## Actions Available
1. **Go to Box (GOTOBOX):** The monkey moves to the location of the box.
2. **Push Box to Banana (PUSHBOX):** If the monkey is at the box’s location, it can push the box to the banana’s location.
3. **Climb Box (CLIMBBOX):** The monkey climbs on top of the box if it's at the same location as the box.
4. **Grab Banana (GRAB):** If the monkey is on the box and at the banana’s location, it can grab the banana.



# Solution


