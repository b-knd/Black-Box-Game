# Coursework 1: Black Box Game

October 26, 2022 8:59 PM 

## How to Run on GHCI Shell

1. Navigate to the folder with the script
2. `:load blackBoxSolver.hs` and type `main`
3. Modify the variable in main to try with different combination of input

![Screenshot 2022-11-06 at 1.46.27 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.46.27_PM.png)

## Challenge 1: Calculate All Interactions [50 Marks]

### Brainstorming and logic planning

******************************Interaction priorities and logic******************************

1. If can be absorbed by any atoms, straightaway return Marking Absorb
2. If can be reflected (including edge reflection), return Marking Reflect
3. Otherwise, deflect accordingly (recursion) otherwise return path

**********************************************************************************************************************************************************************************************************Condition for interference to take effect on a ray (given it’s coordinate and direction of movement)**********************************************************************************************************************************************************************************************************

1. If going towards South from coordinate (a,b), any interference with x== a and y ≥ b will have effect (Any interferences located below the ray’s current position) 
2. If going towards East from coordinate (a, b), any interference with x ≥ a and y == b will have effect (Any interferences located on the right side of ray’s current position)
3. If going towards North from coordinate (a,b), any interference with x == a and y ≤ b will have effect (Any interferences located on above the ray’s current position)
4. If going towards West from coordinate (a,b), any interference with x ≤ a and y == b will have effect (Any interferences located on the left side of ray’s current position)

Example:

![IMG_313CDCFBB9C5-1.jpeg](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/IMG_313CDCFBB9C5-1.jpeg)

******************************************Deflection rule/logic******************************************

Moving towards South or North

- Deflected West when meeting left corners
- Deflected East when meeting right corners
- If meet both left and right, ray moving South will be deflected by topmost interference whereas ray moving North will be deflected by bottommost interference

Moving towards East or West

- Deflected South when meeting upper corners
- Deflected North when meeting lower corners
- If meet both, ray moving East will be deflected by leftmost interference whereas ray moving West will be deflected by rightmost interference

![IMG_A2638C80CCA3-1.jpeg](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/IMG_A2638C80CCA3-1.jpeg)

### Explanation about source code

![Screenshot 2022-11-06 at 1.13.40 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.13.40_PM.png)

![Screenshot 2022-11-07 at 3.02.22 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-07_at_3.02.22_PM.png)

![Screenshot 2022-11-06 at 1.14.19 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.14.19_PM.png)

<aside>
💡 ********Brief explanation of the main code********

Using list comprehension to generate a list of interactions by calling the function intereaction on all possible edge pos.

When receiving an input of edge pos, we first check for absorption as it has the highest priority. Then, check for reflection (including edge reflection). It there is any, return marking straightaway.

Otherwise, deflect rays accordingly and recursively check for interaction for ray’s new current position. The recursion stops when ray is either absorbed or reflected or meeting none interference anymore (can straightaway exit the grid)

</aside>

1. Basic definition of types and data
    
    ```haskell
    -- direction relative to the center of the grid
    data Side = North | East | South | West | All deriving (Show, Eq, Ord) 
    
    -- outcomes of firing rays into grid
    data Marking = Absorb | Reflect | Path EdgePos deriving (Show, Eq)
    
    -- representing coordinate in the form of (col, row)
    type Pos = (Int, Int) 
    
    -- representing entry / exit points to the grid
    type EdgePos = (Side, Int) 
    
    -- representing location of the hidden atoms
    type Atoms = [Pos] 
    
    -- list of outcomes of firing rays into grid
    type Interactions = [(EdgePos, Marking)] 
    
    -- current position with an extra information of Side representing the direction the ray is heading
    type CurrPos = (Side, Pos) 
    ```
    
    - ****************************type CurrPos**************************** is introduced to better keep track of ray movement in the grid
        
        Eg. CurrPos of **(North, (3,5))** means the ray is currently at the coordinate of (3,5) heading towards North side of the grid
        
2. Some helper functions
    1. Miscellaneous
        - **safeMin** and **safeMax** - to avoid getting error when encounter empty list
            
            I’m using it to compare coordinates. In order to ignore result if there is empty list I simply result a minimum coordinate for safemax and return a huge coordinate if there is empty list for safemin.
            
            ![Screenshot 2022-10-30 at 12.46.47 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_12.46.47_PM.png)
            
        - ****************************checkDuplicate**************************** - to see if there is any specific duplicated element found in a given list
            
            ![Screenshot 2022-10-30 at 12.47.18 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_12.47.18_PM.png)
            
    2. Important helper functions
        - ********************genCorners******************** - to generate a list of corners given a list of atoms
            
            ![Screenshot 2022-10-30 at 12.48.01 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_12.48.01_PM.png)
            
            eg. ************genCorners West [(2,3)]************ generates left corners of (2,3) while ****************genCorners North [(2,3)]**************** generates upper corners of (2,3)
            
            ![Screenshot 2022-10-30 at 12.26.55 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_12.26.55_PM.png)
            
        - ********************************interferences -******************************** Generating interferences that will cause effect on the ray based on the ray’s current position and direction of movement
            
            ![Screenshot 2022-10-30 at 12.48.32 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_12.48.32_PM.png)
            
            eg. The ray is moving South in the 5th column, so it will only meet atoms at the 5th columns which are (5,2), (5,5) and (5,7). But since it is already in the 3rd row so atoms above it (5,2) will not have effect and the result is simply [(5,5) and (5,7)].
            
            ![Screenshot 2022-10-30 at 12.35.18 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_12.35.18_PM.png)
            
        - **************deflect************** - the two parameters are direction of deflection and Pos of interference that causes the deflection, returning new position of the ray in the grid
            
            ![Screenshot 2022-10-30 at 12.48.54 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_12.48.54_PM.png)
            
            ![Screenshot 2022-10-30 at 12.40.37 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_12.40.37_PM.png)
            
        - **calcFinal -** converting from currPos to EdgePos
            
            ************************calcStarting -************************ converting from EdgePos to currPos
            
            ![Screenshot 2022-10-30 at 12.49.22 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_12.49.22_PM.png)
            
            eg. Given (North, 1) to generate currPos, the ray is currently at (1, 0) heading South so result is (South, (1,0))
            
            eg. Given (South, (1, 0)), the ray is currently at (1,0) heading South, so it’s final position in the grid will be at the South column 1
            
            ![Screenshot 2022-10-30 at 12.43.14 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_12.43.14_PM.png)
            
3. Checking for absorption or reflection
    - ****************************checkAbsorb****************************
        - If the first interference met by ray is atom then will be absorbed
        - If the ray meet a deflection by there is an atom right after the deflection interference then the ray will be absorbed
        - Otherwise, the ray will not be absorbed
    - ******************************checkReflect -****************************** if ray meet any overlapping interference will be reflected (apply checkDuplicate function to find duplicated interference)
        - Ray heading North will meet bottomost interference and heading West will meet rightmost interference so to select interference use **************minimum************** (both bottomost and rightmost will have greater coordinates)
        - Ray heading South will meet topmost interference and heading East will meet leftmost interference so to select interference use ******************maximum****************** (topmost and leftmost will have smaller coordinates)
    - ********************checkEdgeReflect -******************** if ray has same coordinate with any interference when starting, edge reflection will occur
        
        ![Screenshot 2022-11-07 at 3.05.50 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-07_at_3.05.50_PM.png)
        
4. Deflecting an atoms accordingly
    - To find topmost and leftmost interference will use safemin function, bottomost and rightmost will use safemax function
    - Then deflect ray accordingly based on the deflection rule
        
        ![Screenshot 2022-10-30 at 1.09.12 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_1.09.12_PM.png)
        
        eg. Ray moving South from (3,0) will be deflected East by topmost right corner of atom (2,3) which is (3,4) in this case, ray now move towards East and have new position of (4,2) on the grid
        
        ![Screenshot 2022-10-30 at 1.13.42 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_1.13.42_PM.png)
        
5. Single interaction
    - Priorities: checkAbsorb → checkReflect → checkEdgeReflection → deflection (recursion) → returning final marking
        - Base case for recursion: being absorbed, being reflected or no more interferences found
        
        ![Screenshot 2022-10-30 at 1.15.43 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_1.15.43_PM.png)
        
6. Building final interaction list
    - Concept: list comprehension
        
        ![Screenshot 2022-10-30 at 1.19.48 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_1.19.48_PM.png)
        

### Running on GHCI shell and Output Screenshot

![Screenshot 2022-10-30 at 1.22.54 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-10-30_at_1.22.54_PM.png)

## Challenge 2: Solve a Black Box [50 Marks]

### Explanation about the source code

![Screenshot 2022-11-06 at 1.16.46 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.16.46_PM.png)

<aside>
💡 **************************************Brief explanation of the main code**************************************

I’m basically using brute force approach where I would first

1. Generate a list of possible position of atoms in the entire grid (using combination) 
2. Then I call the calcBBInteractions function on each atom position in the list
3. I compare the interaction produce by those position with the actual interaction input and eliminate those that have incorrent interaction output and return the filtered list
</aside>

1. Some important functions
    - **************************combination:************************** find combination of possible atom’s position of size n
        
        [Function to generate the unique combinations of a list in Haskell](https://stackoverflow.com/questions/52602474/function-to-generate-the-unique-combinations-of-a-list-in-haskell)
        
        ![Screenshot 2022-11-06 at 1.17.52 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.17.52_PM.png)
        
        ![Screenshot 2022-11-06 at 1.19.07 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.19.07_PM.png)
        
    - ******************genGrid:****************** generate grid of size n
        
        ![Screenshot 2022-11-06 at 1.19.32 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.19.32_PM.png)
        
        ![Screenshot 2022-11-06 at 1.19.46 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.19.46_PM.png)
        
    - ********************************checkInteraction:******************************** compare two list of interactions to check if first list is subset of the second list using recursion
        
        ![Screenshot 2022-11-06 at 1.21.06 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.21.06_PM.png)
        
        ![Screenshot 2022-11-06 at 1.21.58 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.21.58_PM.png)
        
    - **************filterCombination:************** use checkInteraction to filter unwanted atom’s position
        - If the interaction produced by the atom’s position does not match the desired interactions, remove from list (do not append to result)
        
        ![Screenshot 2022-11-06 at 1.23.43 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.23.43_PM.png)
        
    - ******************findMax:****************** find the maximum possible grid size from the input interaction
        - Eg. [((North, 1), Absorb), ((West, 8), Reflect)] return 8
        
        ![Screenshot 2022-11-06 at 1.24.11 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.24.11_PM.png)
        
        ![Screenshot 2022-11-06 at 1.26.16 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.26.16_PM.png)
        
    - **************************extractPath:************************** From (Path, (North, 3)) extract 3
        
        ![Screenshot 2022-11-06 at 1.27.08 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.27.08_PM.png)
        
    1. Final main function
        
        ![Screenshot 2022-11-06 at 1.31.43 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.31.43_PM.png)
        

### Running on GHCI shell and Output Screenshots

Took a while to generate possible position for the interaction in part 1 due to use of brute force approach. But for small list and small grid size, possible atom’s position could be identified.

![Screenshot 2022-11-06 at 1.37.05 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-06_at_1.37.05_PM.png)

![Screenshot 2022-11-07 at 4.50.26 PM.png](Coursework%201%20Black%20Box%20Game%20cd7e64beeb6b45f3aba28a5cc50706aa/Screenshot_2022-11-07_at_4.50.26_PM.png)