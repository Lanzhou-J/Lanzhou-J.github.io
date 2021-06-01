---
title: OOP style Minesweeper console game written in C#
layout: post
subtitle: null
date: '2021-06-01 16:58:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- English
- project
- study
- work
---

I made a simple object-oriented Minesweeper console app (a hardcore old school PC game).

What have I learned in this project? I listed some of the main learning content during this time, including foundational knowledge such as Git, c# basics, Four Rules of Simple Design, SOLID, TDD etc. 

Back to the kata, this is the classic Minesweeper game that people are familiar with - It is a logic game, where the goal is to sweep for mines, and you do this by clicking the squares on the grid. The goal is to “reveal” all safe squares without uncovering a mine and blowing up the entire board. All the mines are hidden when you start the game. So you need to use logic to determine where the mines are and avoid clicking on these mine squares. These coloured numbers are the hints that you can get from the game. Each number indicates the total number of mines within the surrounding neighbour squares.

![classic Minesweeper game](/img/in-post/Minesweeper.png)
If the centre square is not on the edge of the board, then it has 8 neighbours; If it is on the edge but not on the corner of the board, then it has 5 neighbours; if the square is on the corner, it has 3 neighbours.

Next let’s have a look at the specific requirements of this kata, the purpose of this kata is to build a Minesweeper console game. The original title of this kata is “Minesweeper for testing” and the purpose of this exercise is to understand how to build the core of a system without any IO. I’ll explain my testing strategy later. First thing first let’s go through the requirements. I have separated requirements into 2 parts. The first part is about how to set up the game, as well as how to display the board to users. Let's go through the requirements - 
- A new game should start by specifying the difficulty of the game (all boards are squares)e.g. if board has a difficulty of 4, the board has a width of 4 and a height of 4 and 4 mines
- Mines should be placed randomly
- The board should have two modes of display
- Only squares revealed by the player are displayed
- The entire revealed board is displayed

The second part is about how to actually play the game, and how to determine win or lose.
- A player should be able to select the next square to reveal
- If the chosen square is a mine, the game is over, and the player loses
	- When the player loses, the entire revealed board is displayed
- If the chosen square is not a mine, that square is revealed with the number of mines that surround it, these number are the hints
- If all the squares are revealed except mines, the player wins

Here is a game flow example: A new game started with specified difficulty. Player would be asked to input a difficulty value, e.g. 4, then a 4 * 4 hidden state board will be displayed to the player. And in the end, when game is over, either winning or losing, the board will entirely reveal and be displayed to the user. The result of the game will be displayed to the user as well. 

Flowchart:

![flowchart](/img/in-post/Minesweeper-Flowchart3.png)

This is the flowchart of the program I designed, similarly, it can be divided into two stages, the first stage: generating a board that can be used by the game; the second stage, the process of playing the game and  determine win or lose. How was this board created? First, an empty board is generated according to the user input, and then MinesGenerator will randomly place mines on the board, and then the HintsGenerator will loop through all the mine squares and then change the Hint value of the surrounding squares according to the position of Mines, and finally show the user the game board in hidden mode. 

Next, A while loop is used here in the second stage whey user starts to play the game, this condition: "Board is not entirely revealed" is evaluated before processing the body of the loop. When the condition is true, the body will be executed. game collects user input of Location value, then update board by revealing the corresponding square, then WinLoseChecker will check the current Board. If the current state of the Board matches the losing condition, then we lose the game, then reveal the entire board, display board , end of loop; Similarly if current Board matches winning condition, we win the game, reveal the entire board, display board, end of loop. If neither condition is true, then loop continues.

Before we dive into code detail, I would like to show you the big picture of my design.

Class Diagram:
![class diagram](/img/in-post/Minesweeper-Rule2.png)

This picture looks a bit complicated at first glance, so I also divided it into different parts. The first part is the core system of the program, and the second part on the edge is responsible for IO - input and output . The core system is mainly composed of four classes, namely Location, Square, Board and Game, which is the overarching orchestrator. These other classes are my helper classes, they provide services for the program. 

In the process of development, I strictly follow the TDD development cycle, Write a failing test ( the test bar is red), Write production code to make the test pass (then the test bar turn green), Refactor, Repeat. The programming strategy I choose is Bottom-Up approach, I started with details and worked my way up to a complete system. The reason I chose this strategy is that when I started programming, I didn’t know how to solve this problem, I don't know anything about the core business logic, but I know what the game would look like? From the user's point of view, this game will have a board composed of squares. This is why the first test I wrote at the very beginning was about how to create a board. Then how to create a square. I start from the entities.
After having square, board and location, the logic in the game class or the logic in MineGenerator, HintGenerator seems to be generated naturally. If you ask me which is the most important category in this system, then I think it is Square, because this category is the smallest unit in the system, it is like the foundation of a building, it is the beginning of the whole story, 
---
Now let's have a look at my code.
then we Now focus on the Square class. Square has IsRevealed property to represent hidden or revealed, and IsMine boolean to represent if it is a safe square or a mine square. It has a Location, and a hint value. Why should I extract this Location class?

1. I use List<Square> in my board class instead of jagged array or 2-d array, I need something to represent each of the Square's identity and location on the board since I don't have indexes. (The reason why I choose List over jagged array or 2-d array: At first I chose 2d array because it used index naturally to reference points in the structure, these indexes can be seen as coordinates and it all comes naturally; However, I chose List of Squares over 2d array because it is easier for me to do Linq queries on it, and achieve my goals easily such as place mines on the board. )

2. One benefit of Location class, is that gives us a centralized place for our topology knowledge. When you need to use the concept of a location in the system e.g. as argument , you don't need to use 2 values xValue and yValue, you can replace them with one argument: location. It reveals the intention clearly.

Program.cs is the entry point of my program, in order to construct a game, I need Input, Output, MinesGenerator. You may ask, why do you use interface here? My main purpose is for testing, random mines are hard to test because you don’t know their specific location, but I can use test double, a mockMineGenerator instead of RandomMineGenerator, so that I can accurately predict the location of Mines and create test cases to distinguish losing and winning conditions. Input and Output use interface for similar reasons. One for testing, I can use test double like MockInput instead of ConsoleInput for my user acceptance test, it also makes my program more flexible. For example, it allows me to easily use an IO system that is not a console in the future.

Now we have seen what the entry point looks like, it will lead you to Game class. CreateBoard function and Play function. I remember my mentors mentioned that Clean code should look like a newspaper, You read most important information first in function names just like a title, then detail of code implementation. And I have tried my best to improve code readability. Let's take CreateBoard as an example. it set up the board, initialise game. It starts by CreateEmptyBoard based on size, then ... Then if you are interested to know more about how mines are generated or how hints are generated, you can continue reading the code in MineGenerator or HintGenerator.
