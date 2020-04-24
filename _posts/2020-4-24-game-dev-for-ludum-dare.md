---
layout: post
title: "Writing a Game in C"
---
## Ludum Dare Adventures

This past weekend, [Ludum Dare](https://ldjam.com/), one of the internets longest running game jams, took place. Contestants use the weekend to code a game from scratch, including art, music and everything else. I had never coded a whole game before and I had some extra free time, so I decided to challenge myself and join the contest.

## Getting Started 

Ludum Dare has two different types of competition, the compo and the jam. In the compo, a game has to be coded entirely from scratch in 48 hours by a single individual. In order to satisfy the requirements, all of the game's assets have to be made from scratch in the 48 hour period as well. The jam is more lenient. You can use existing code and you have 72 hours to finish your game. Additionally, you have the ability to work with a team.

I don't have much experience with game development, and I have even less with digital art so I decided to recruit my brothers to help design the artwork and music for the game. When the competition started we all hopped on a call to discuss our ideas and create the assets. This was probably the most exciting part of the game jam and progress came quickly.


## The Idea

The theme for the contest was "Keep it Alive". We decided to make a top down adventure game where a hero has to run away from ravenous dinosaurs to survive. My brothers quickly created some very cool art and [music](/images/ChaseMusic.ogg) to match the theme.


![Dino 1](/images/dino1.png)
![Dino 2](/images/dino2.png)
![Dino 3](/images/dino3.png)
![Dino 4](/images/dino4.png)


![Player 1](/images/PlayerCharacter.png)
![Player 2](/images/PlayerCharacter1.png)
![Player 3](/images/PlayerCharacter2.png)
![Player 4](/images/PlayerCharacter3.png)

## Coding

### C and Raylib
I decided to use C to develop the game for two reasons:

* I use C a lot for classes and wanted to use a language that I was familiar with for the jam. 

* I wanted to challenge myself to make a game using a lower level language.

This ended up being a lot of fun, but also quite challenging. Oftentimes I found myself thinking about the best way
to manage memory or store sprites rather than working on game mechanics but it was quite instructional!

To make things a bit easier on myself I decided to use a library called [raylib](https://www.raylib.com/) to help my development. Raylib exposes a simple API for creating windows and drawing things to the screen.

### Project Structure and Workflow

I used Vim for an editor and setup a simple Makefile to compile my code. All of the source files lived in a /src directory, and I kept the header files in a separate /include directory. All of the assets lived in a separate directory as well. This seemed to work well.

![Workflow](/images/vim_workflow.png)

### Challenges

The game dev was fun but not without obstacles. This was the first time I had used raylib so it took a while to learn how to use the library. For example, resizing a sprite involved loading the texture, transforming it into an image, and then resizing it, before turning it back into a texture for rendering. Using C also probably slowed down the development since I spent a decent amount of time on structuring memory instead of coding gameplay features. Additionally, I had to duplicate a decent bit of code between the player struct and the dino structs. In the future I would probably attempt to encapsulate entity behavior to simplify the code and speed up development.

The other big challenge was deploying the game to different platforms. I was developing on a Mac so compiling for that platform was not too difficult. I didn't have access to a Windows PC during the jam so I was unable to compile the game for Windows. Hopefully I can get that working in the future! Choosing a language that allows easy cross-platform compliation will definitely be a priority for future endeavors.

## The Game 

The final result of all of this work looks like this!

![Dino Ridge](/images/dino_screenshot.png)

The player character can run around in an effort to escape the dinosaurs. Each dinosaur has its own way of chasing the player as well. Some are territorial, some run around randomly, and one always chases after you. If your health reaches 0 its game over. The goal is to get the highest score, achieved by staying alive for a long period of time.


If you're interested in the source code for the game, it can be found [here!](https://github.com/ConnorBach/DinoRidge)
