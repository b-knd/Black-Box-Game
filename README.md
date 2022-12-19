‼️ **Please do not fork without permission** ‼️

# Black-Box-Game

## 📌 About
This is a programming coursework completed during my Year 2 in [BSc. Computer Science, University of Southampton Malaysia](https://www.southampton.ac.uk/my/undergraduate/courses/bsc-computer-science.page)

**Module:** [COMP2209 Programming III](https://www.southampton.ac.uk/courses/modules/comp2209)

This repository contains
- [**my solution code**](./blackBoxSolver.hs)</br>
- [**introduction to black box game**](#-introduction-to-black-box-game)</br>
- [**coursework specification**](#-coursework-1--comp2209-programming-iii)

---

## 💡 Introduction to Black Box Game
Some resources: </br>
- [Black Box (game)](https://en.wikipedia.org/wiki/Black_Box_(game))</br>
- [Black Box - a Paper & Pencil deductive strategy game for 2 players (Pen and Paper)
](https://www.youtube.com/watch?v=aF9OU1_Bi4g)



## 👩🏻‍💻 Coursework 1 – COMP2209 Programming III 

Prepared by Dr. Kristo Radion Purba (University of Southampton, Malaysia), adapted from Coursework sheet by Dr. Julian Rathke and Dr. Nick Gibbins (Southampton UK).

**📑 Learning Outcomes (LO)** Understand the concept of functional programming and be able to write programs in this style

**📚 Effort**  15 to 30 hours

**💯 Weighting** 20% of module mark


### Challenge 1: Calculate All Interactions [50 Marks]
The first challenge requires you to define a function
**calcBBInteractions :: :: Int -> Atoms -> Interactions**
that, given an integer representing the NxN size of the grid (not including edges) and a list of atoms placed within the grid, returns the set of interactions from all possible edge entry points. That is, the order of the interactions returned is not important and there should be no duplicate entries starting at any edge point. For the example board given above the function should return:
```
calcBBInteractions 8 [ (2,3) , (7,3) , (4,6) , (7,8) ] =

[((North,1),Path (West,2)),((North,2),Absorb), ((North,3),Path (North,6)),((North,4),Absorb), 
((North,5),Path (East,5)),((North,6),Path (North,3)), ((North,7),Absorb),((North,8),Path (East,2)), 
((East,1),Path (West,1)),((East,2),Path (North,8)), ((East,3),Absorb),((East,4),Path (East,7)), 
((East,5),Path (North,5)),((East,6),Absorb), ((East,7),Path (East,4)),((East,8),Absorb),
((South,1),Path (West,4)),((South,2),Absorb), ((South,3),Path (West,7)),((South,4),Absorb), 
((South,5),Path (West,5)),((South,6),Reflect), ((South,7),Absorb),((South,8),Reflect), 
((West,1),Path (East,1)),((West,2),Path (North,1)), ((West,3),Absorb),((West,4),Path (South,1)), 
((West,5),Path (South,5)),((West,6),Absorb), ((West,7),Path (South,3)),((West,8),Absorb)]
```
**Marking Criteria:**
- 5: Flexibility in accepting any size of grid N
- 5: Ray can be absorbed
- 5: Ray can be deflected correctly
- 5: Ray can be reflected correctly, including edge reflections
- 5: Building a single Interaction correctly
- 5: Building the final Interactions list
- 10: Clarity and conciseness of the explanations in the report
- 10: Overall codes efficiency (less codes, less recursion, less redundant) and readability

### Challenge 2: Solve a Black Box [50 Marks]
This challenge requires you to define a function
**solveBB :: Int -> Interactions -> Atoms**
that, given an integer N, and a list of the outcomes of firing rays into the black box, returns a list of the positions of exactly N atoms that gives rise to the given list of interactions.
Where no such list exists, you should return the empty list.
Where multiple possible placements exist then you should return all of them. The order in which the positions of the atoms are listed is also unimportant.
For example, when called with the integer value 4 and the output from the example given in Challenge 1 above, your function should return ``[ (2,3) , (7,3) , (4,6) , (7,8) ]``

**Marking Criteria:**
- 5: Flexibility in accepting any size of grid N
- 5: Modularity; the ability to reuse existing support functions from Challenge 1
- 5: Identifying possible Atoms positions correctly
- 5: Building the final Atoms list
- 10: Ability to produce all possible Atom positions regardless of the completeness of the input. For
example, if we pass only 2 Interactions, and the program will return all the possible Atom positions.
- 10: Clarity and conciseness of the explanations in the report
- 10: Overall codes efficiency (less codes, less recursion, less redundant) and readability
