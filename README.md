# Introduction

Ǹolaniirrarat [ŋo̞̼.lä<sup>β</sup>.niːɹ.ɹä<sup>β</sup>.ɹä<sup>β</sup>t] is Koiné Gilis for "Odd Cyclic Weapons."
The traditional game of rock paper scissors only has 3 weapons. 
Have you ever wondered how a variant with more than 3 weapons work, but are too lazy to compute the strength and weaknesses?
Well wonder no longer, since this code generalizes the game to any variant that has a fair strengths and weaknesses chart (SWC) up to rotation.

Okay, what does a "fair SW chart" mean? 
A rock-paper-scissors variant must have:
* an odd number, that's at least 3, of weapons.
* each weapon must be strong against k weapons and weak against the remaining k weapons.

The "up to rotation" part will be explained later.

Throughtout the guide, SW Labeled Adjacency Matrices (SWLAMs) will be used. Why? Because explaining how the variants work will be so much easier. SWLAMs are read as: row verdict column

The SWLAM of the traditional rock-paper-scissors:

　           | Rock | Paper | Scissors
------------ | ---- | ----- | --------
**Rock**     | D	   | L	    | W
**Paper**    | W	   | D	    | L
**Scissors** | L	   | W	    | D


The SWLAM of the other variant:

　           | Paper | Rock | Scissors
------------ | ---- | ----- | --------
**Paper**    | D    | W     | L
**rock**     | L    | D     | W
**Scissors** | W    | L     | D


As is, this SWLAM describes the same variant as the above SWLAM, so relabelling the column and row headers is needed:

　           | Rock | Paper | Scissors
------------ | ---- | ----- | --------
**Rock**     | D    | W     | L
**Paper**    | L    | D     | W
**Scissors** | W    | L     | D

The above SWLAM doesn't really make any sense to the average person. 
So another relabelling is needed. 
What's the most neutral way to name categories? 
With numbers of course! 
So relabelling the weapons to have numbers as names:

　    | 1 | 2 | 3
----- | - | - | -
**1** | D | W | L
**2** | L | D | W
**3** | W | L | D

An example with 5 weapons:

　    | 1 | 2 | 3 | 4 | 5
----- | - | - | - | - | -
**1** | D | W | W | L | L
**2** | L | D | W | W | L
**3** | L | L | D | W | W
**4** | W | L | L | D | W
**5** | W | W | L | L | D

The above is the "base" 5-weapon SWLAM. 
As discussed earlier, each weapon is weak to 2 other and strong against the remaining 2.
If the above algorithm is applied, this gives a new but fair SWLAM:

　    | 1 | 2 | 3 | 4 | 5
----- | - | - | - | - | -
**1** | D | L | W | W | L
**2** | W | D | W | L | L
**3** | L | L | D | W | W
**4** | L | W | L | D | W
**5** | W | W | L | L | D

But, unfortunately, some information was lost. 
The variant code isn't readily known anymore. It has to be calculated for. 
To fix this:

　    | 　    |  1  |  2  |  3  |  4  |  5
----- | ----- | --- | --- | --- | --- | ---
　    | 　    | *2* | *1* | *3* | *4* | *5*
**1** | *2*   |  D  |  L  |  W  |  W  |  L
**2** | *1*   |  W  |  D  |  W  |  L  |  L
**3** | *3*   |  L  |  L  |  D  |  W  |  W
**4** | *4*   |  L  |  W  |  L  |  D  |  W
**5** | *5*   |  W  |  W  |  L  |  L  |  D

The column and row in bold are the weapon names, while the column and row in italics are the variant code.

Now, given this, there may seem to be 5! variations, but no there are actually only 4! variations, since each variation has 5 rotations.
For example: 12345 and 23451 are the same SWLAM because they're rotations of each other.
Think of it as rotating the SWC by one vertex.
So in general, for n weapons, there are (n-1)! SWLAM variations.

Now, why is a SWLAM an "adjacency matrix"? 
Well, if the only portion with the Ws, Ds, and Ls are taken:

　 | 　 | 　 | 　 | 　
--- | --- | --- | --- | ---
 D  |  L  |  W  |  W  |  L
 W  |  D  |  W  |  L  |  L
 L  |  L  |  D  |  W  |  W
 L  |  W  |  L  |  D  |  W
 W  |  W  |  L  |  L  |  D

And relabel them such that:
* W → 1
* D → 0
* L → -1

　 | 　 | 　 | 　 | 　
-- | -- | -- | -- | --
0  | -1 | 1  | 1  | -1
1  | 0  | 1  | -1 | -1
-1 | -1 | 0  | 1  | 1
-1 | 1  | -1 | 0  | 1
1  | 1  | -1 | -1 | 0

Then this decribes a directed graph. Specifically the SWC for the respective variant code.

From this paper: https://chamberland.math.grinnell.edu/papers/rps.pdf, when the number of weapons is at least 7, the method above can only generate the variants that are isomorphic to one of the trivial automorphism groups. In simpler terms, the above method is very limited. 

# Patch Notes
## 1.0 
* Initial Commit

## 1.1 
* Fixed the SW Chart
* Refactored the code such that the string form of the variant codes is calculated from the list form

## 1.2
* Made the random generation of new variant codes much faster
* Functions that are able now use the Numba decorator @jit with nopython set to True to speed up
* Please ignore Numba's deprecation warnings about reflected lists

## 1.3
* Solved the useless warnings
* Increased the display size of the SWC
* Added the choice to change the amount of weapons
* Added Player class, but is unused

## 1.4
* The Player class is now used in judge() and other places

# USAGE
## Offline
1. Install Python and Jupyter Notebook.
2. Make sure your Python version is at least 3.9
3. Make sure the following packages are installed:
    * numpy
    * matplotlib
    * networkx
    * pandas
    * random
    * numba
4. Download Ǹolaniirrarat.ipynb from this repository. 
5. Run the Notebook. The Notebook will guide you through the step by step process.

## Online
1. Download Ǹolaniirrarat.ipynb from this repository
2. Go to https://colab.research.google.com/notebooks/intro.ipynb#recent=true
3. On the Upload tab, navigatate to where the notebook was saved in your machine.
4. Run the Notebook. The Notebook will guide you through the step by step process.

# TO-DO

1. Allow renaming of weapons and rewording of how each beat one another 
    * Allow for this template: \[weapon 1\] \[method\] \[weapon 2\]
        * Concrete example: Rock blunts Scissors
3. Make a better UI
4. Find a way to allow the players to access the variants outside of the current automorphism group
    * Formulate a naming scheme, if a way is found
