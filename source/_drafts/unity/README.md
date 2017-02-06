# Unity Tutorials

![Unity Tutorials](_asset/image/banner.jpg)

<!-- TOC -->

- [Unity Tutorials](#unity-tutorials)
    - [Engine](#engine)
        - [Projects](#projects)
            - [Roll-a-ball tutorial (9/9)](#roll-a-ball-tutorial-99)
                - [Introduction](#introduction)
                - [Environment and Player](#environment-and-player)
                - [Camera and Play Area](#camera-and-play-area)
                - [Collecting, Scoring and Building the game](#collecting-scoring-and-building-the-game)
            - [Space Shooter tutorial (17/19)](#space-shooter-tutorial-1719)
                - [Introduction](#introduction)
                - [1. Game setup, Player and Camera](#1-game-setup-player-and-camera)
                - [2. Boundaries, Hazards and Enemies](#2-boundaries-hazards-and-enemies)
                - [3. Scoring, Finishing and building the game](#3-scoring-finishing-and-building-the-game)
                - [4. Extending Space Shooter](#4-extending-space-shooter)
            - [Survival Shooter tutorial (0/12)](#survival-shooter-tutorial-012)
                - [Training Day Phases](#training-day-phases)
                - [Upgrading Audio to Unity 5](#upgrading-audio-to-unity-5)
            - [Tanks tutorial (0/8)](#tanks-tutorial-08)
                - [Training Day Phases](#training-day-phases)
            - [2D Roguelike tutorial (0/14)](#2d-roguelike-tutorial-014)
                - [1. Setup and Assets](#1-setup-and-assets)
                - [2. Level Generation](#2-level-generation)
                - [3. Unit Mechanics](#3-unit-mechanics)
                - [4. Architecture and Polish](#4-architecture-and-polish)
            - [2D UFO Tutorial (9/9)](#2d-ufo-tutorial-99)
                - [Introduction & Setup](#introduction--setup)
                - [Movement Basics](#movement-basics)
                - [Collectable Objects](#collectable-objects)
                - [Creating a Playable Build](#creating-a-playable-build)
            - [Adventure Game Tutorial (0/7)](#adventure-game-tutorial-07)
                - [Training Day Phases](#training-day-phases)
            - [Let's Try Assignments (0/12)](#lets-try-assignments-012)
                - [Let's Try Assignments](#lets-try-assignments)
                - [Mini-Projects from the Live Training Archive](#mini-projects-from-the-live-training-archive)
            - [Procedural Cave Generation tutorial (0/9)](#procedural-cave-generation-tutorial-09)
                - [1. Generating Content](#1-generating-content)
                - [2. Walls and Rooms](#2-walls-and-rooms)
                - [3. Polish and Passageways](#3-polish-and-passageways)
        - [Topics](#topics)
            - [Interface & Essentials (6/22)](#interface--essentials-622)
                - [Using the Unity Interface](#using-the-unity-interface)
                - [Essential Unity Concepts](#essential-unity-concepts)
                - [Extending the Unity Editor](#extending-the-unity-editor)
                - [Live Sessions on Unity Interface and Essentials](#live-sessions-on-unity-interface-and-essentials)
            - [2D Game Creation (0/37)](#2d-game-creation-037)
                - [Unity for 2D](#unity-for-2d)
                - [Live Sessions on 2D](#live-sessions-on-2d)
                - [Live Session: Making A Flappy Bird Style Game](#live-session-making-a-flappy-bird-style-game)
                - [Community Extensions](#community-extensions)
            - [Scripting (0/94)](#scripting-094)
                - [Beginner Gameplay Scripting](#beginner-gameplay-scripting)
                - [Intermediate Gameplay Scripting](#intermediate-gameplay-scripting)
                - [Community Posts](#community-posts)
                - [Project Architecture](#project-architecture)
                - [Getting Started with Unity Development using Visual Studio](#getting-started-with-unity-development-using-visual-studio)
                - [Live Sessions on Scripting](#live-sessions-on-scripting)
                - [Live Session: Quiz Game 1](#live-session-quiz-game-1)
                - [Live Session: Quiz Game 2](#live-session-quiz-game-2)
            - [Best Practices (0/12)](#best-practices-012)
                - [Assets, Resources and AssetBundles](#assets-resources-and-assetbundles)
                - [Optimizing Unity UI](#optimizing-unity-ui)
            - [Graphics (62)](#graphics-62)
                - [Introduction to Lighting and Rendering](#introduction-to-lighting-and-rendering)
                - [Precomputed Realtime GI](#precomputed-realtime-gi)
                - [Rendering and Shading](#rendering-and-shading)
                - [Cameras and Effects](#cameras-and-effects)
                - [Geometry in Unity](#geometry-in-unity)
                - [Live Sessions on Graphics](#live-sessions-on-graphics)
                - [Unite Presentations](#unite-presentations)
                - [Substance (by Allegorithmic)](#substance-by-allegorithmic)
            - [Physics (24)](#physics-24)
                - [3D Physics](#3d-physics)
                - [2D Physics](#2d-physics)
                - [Assignments](#assignments)
                - [Live Training On Physics](#live-training-on-physics)
                - [Articles](#articles)
            - [Audio (12)](#audio-12)
                - [Audio Setup](#audio-setup)
                - [Audio Mixing](#audio-mixing)
                - [Live Trainings on Audio](#live-trainings-on-audio)
                - [Upgrading Audio to Unity 5](#upgrading-audio-to-unity-5)
            - [Animation (18)](#animation-18)
                - [Animating](#animating)
                - [Controlling Animation](#controlling-animation)
                - [Character Animation](#character-animation)
                - [Live Sessions on Animation](#live-sessions-on-animation)
                - [Community Posts on Animation](#community-posts-on-animation)
            - [User Interface (UI) (41)](#user-interface-ui-41)
                - [UI Components (11)](#ui-components-11)
                - [Live Sessions On UI (10)](#live-sessions-on-ui-10)
                - [Creating a Tic-Tac-Toe game using only the UI (11)](#creating-a-tic-tac-toe-game-using-only-the-ui-11)
                - [Live Training: Shop UI with Runtime Scroll Lists(9)](#live-training-shop-ui-with-runtime-scroll-lists9)
            - [Mobile & Touch (0/6)](#mobile--touch-06)
                - [Getting Input](#getting-input)
                - [Building](#building)
                - [Mobile optimization](#mobile-optimization)
            - [Navigation (0/7)](#navigation-07)
                - [Navigation Basics](#navigation-basics)
                - [Live Sessions On Navigation](#live-sessions-on-navigation)
            - [Tips (0/19)](#tips-019)
                - [Tips](#tips)
                - [Live Training Sessions](#live-training-sessions)
            - [Ads & Analytics (0/9)](#ads--analytics-09)
                - [Analytics Basics](#analytics-basics)
                - [Unity IAP](#unity-iap)
                - [Unite Presentations](#unite-presentations)
                - [Unity Ads 2.0](#unity-ads-20)
            - [Virtual Reality (0/8)](#virtual-reality-08)
                - [Introduction to VR](#introduction-to-vr)
            - [Performance Optimization (0/4)](#performance-optimization-04)
                - [Diagnosing performance problems](#diagnosing-performance-problems)
                - [Fixing performance problems](#fixing-performance-problems)
            - [Multiplayer Networking (0/21)](#multiplayer-networking-021)
                - [Creating a simple multiplayer example](#creating-a-simple-multiplayer-example)
                - [Live Session: Merry Fragmas 3.0](#live-session-merry-fragmas-30)
    - [Services & Production](#services--production)
        - [Topics](#topics)
            - [Cloud Build (4)](#cloud-build-4)
                - [Lessons](#lessons)
                - [Appendix](#appendix)
            - [The Asset Store (14)](#the-asset-store-14)
                - [Asset Store Basics](#asset-store-basics)
                - [Community Posts](#community-posts)
                - [Community Playlist - Apex Game Tools](#community-playlist---apex-game-tools)
            - [Developer Advice (7)](#developer-advice-7)
                - [Your First Game](#your-first-game)
                - [PR & Marketing](#pr--marketing)
            - [Game Performance Reporting (1)](#game-performance-reporting-1)
                - [Game Performance Reporting](#game-performance-reporting)
            - [Production (2)](#production-2)
                - [Marketing and PR](#marketing-and-pr)

<!-- /TOC -->

## Engine

### Projects

#### Roll-a-ball tutorial (9/9)

Create a simple rolling ball game that teaches you many of the principles of working with Unity.
In your first foray into Unity development, create a simple rolling ball game that teaches you many of the principles of working with Game Objects, Components, Prefabs, Physics and Scripting. No asset download required.

Questions? [Ask in the official forum thread here](http://forum.unity3d.com/threads/roll-a-ball-tutorial-q-a.319451/?_ga=1.185871968.838993178.1480250241).

##### Introduction

* [x] [Introduction to Roll-a-Ball](https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial/introduction-roll-ball?playlist=17141)

##### Environment and Player

* [x] [Setting up the Game](https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial/setting-game?playlist=17141)
* [x] [Moving the Player](https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial/moving-player?playlist=17141)

##### Camera and Play Area

* [x] [Moving the Cameara](https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial/moving-camera?playlist=17141)
* [x] [Setting up the Play Area](https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial/setting-play-area?playlist=17141)

##### Collecting, Scoring and Building the game

* [x] [Creating Collectable Objects](https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial/creating-collectable-objects?playlist=17141)
* [x] [Collecting the Pick Up Objects](https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial/collecting-pick-objects?playlist=17141)


#### Space Shooter tutorial (17/19)

Expand your knowledge and experience of working with Unity by creating a simple top down arcade style shooter.
Using basic assets provided by Unity Technologies, built-in components and writing simple custom code, understand how to work with imported Mesh Models, Audio, Textures and Materials while practicing more complicated Scripting principles. 

[Download the assets for free from the Asset Store here.](https://www.assetstore.unity3d.com/en/content/13866?_ga=1.172689898.838993178.1480250241)
[Download the Upgrade Guide to Unity 5 here.](https://oc.unity3d.com/index.php/s/rna2inqWBBysn6l?_ga=1.172689898.838993178.1480250241)
Questions? [Ask in the official forum thread here.](http://forum.unity3d.com/threads/space-shooter-tutorial-q-a.313899/?_ga=1.172689898.838993178.1480250241)

##### Introduction

* [x] [1. Introduction to Space Shooter](https://unity3d.com/learn/tutorials/projects/space-shooter/introduction?playlist=17147)

##### 1. Game setup, Player and Camera

* [x] [1. Setting up the project](https://unity3d.com/learn/tutorials/projects/space-shooter/setting-up-the-project?playlist=17147)
* [x] [2. The player GameObject](https://unity3d.com/learn/tutorials/projects/space-shooter/the-player-gameobject?playlist=17147)
* [x] [3. Camera and lighting](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/camera-and-lighting?playlist=17147)
* [x] [4. Adding a background](https://unity3d.com/learn/tutorials/projects/space-shooter/adding-a-background?playlist=17147)
* [x] [5. Moving the player](https://unity3d.com/earn/tutorials/projects/space-shooter/moving-the-player?playlist=17147)
* [x] [6. Creating shots](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/creating-shots?playlist=17147)
* [x] [7. Shooting shots](https://unity3d.com/learn/tutorials/projects/space-shooter/shooting-shots?playlist=17147)

##### 2. Boundaries, Hazards and Enemies

* [x] [1. Boundary](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/boundary?playlist=17147)
* [x] [2. Creating hazards](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/creating-hazards?playlist=17147)
* [x] [3. Explosions](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/explosions?playlist=17147)
* [x] [4. Game Controller](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/game-controller?playlist=17147)
* [x] [5. Spawning waves](https://unity3d.com/learn/tutorials/projects/space-shooter/spawning-waves?playlist=17147)

##### 3. Scoring, Finishing and building the game

* [x] [1. Audio](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/audio?playlist=17147)
* [x] [2. Counting points and displaying the score](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/counting-points-and-displaying-score?playlist=17147)
* [x] [3. Ending the game](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/ending-game?playlist=17147)
* [x] [4. Building the game](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/building-game?playlist=17147)


##### 4. Extending Space Shooter

* [ ] [1. Extending Space Shooter: Enemies, More Hazards, Scrolling BG...](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/extending-space-shooter-enemies-more-hazards?playlist=17147)
* [ ] [2. Mobile Development: Converting Space Shooter to Mobile](https://unity3d.com/learn/tutorials/topics/mobile-touch/mobile-development-converting-space-shooter-mobile?playlist=17147)

#### Survival Shooter tutorial (0/12)

Learn how to make an isometric 3D survival shooter game.
Learn how to make an isometric survival shooter game with this project from Unite training day 2014. NOTE: The original project shown in the videos was created with Unity 4.6 and can be followed with that version, but we recommend that you use Unity 5.1 or greater, and read the Upgrade Guide pdf located in the root of the project files.

[Click here to download the 5.x version of the project.](https://www.assetstore.unity3d.com/en/?_ga=1.109907532.838993178.1480250241#!/content/40756)
Questions? [Ask in the official forum thread here.](http://forum.unity3d.com/threads/unity-5-survival-shooter-q-a.338190/?_ga=1.109907532.838993178.1480250241)
You can download the slides [here](https://oc.unity3d.com/index.php/s/xQbGL7Fm3mF0ySs?_ga=1.109907532.838993178.1480250241).

##### Training Day Phases

* [ ] [1. Environment setup]()
* [ ] [2. Player Character]()
* [ ] [3. Camera setup]()
* [ ] [4. Creating Enemy #1]()
* [ ] [5. Health HUD]()
* [ ] [6. Player Health]()
* [ ] [7. Harming Enemies]()
* [ ] [8. Scoring points]()
* [ ] [9. Spawning Enemies]()
* [ ] [10. Game Over]()

##### Upgrading Audio to Unity 5

* [ ] [1. Upgrading Unity 4 Audio Part 1: Mixers and Effects]()
* [ ] [2. Upgrading Unity 4 Audio Part 2: Snapshots and Exposed Parameters]()


#### Tanks tutorial (0/8)

Originally recorded at Unite Boston 2015, this tutorial series will teach you how to create a 2 player (one keyboard) shooter game. You'll learn about simple game mechanics, integrating world and screen space UI, as well as game architecture and audio mixing.

[Download Project Files](https://www.assetstore.unity3d.com/en/?_ga=1.180489582.838993178.1480250241#!/content/46209/)

[Download the Slides as PDF](https://oc.unity3d.com/index.php/s/n6P1VcTa4NWQbyn?_ga=1.180489582.838993178.1480250241)

##### Training Day Phases

* [ ] [1. Scene Setup]()
* [ ] [2. Tank Creation & Control]()
* [ ] [3. Camera Control]()
* [ ] [4. Tank Health]()
* [ ] [5. Shell Creation]()
* [ ] [6. Firing Shells]()
* [ ] [7. Game Managers]()
* [ ] [8. Audio Mixing]()

#### 2D Roguelike tutorial (0/14)

Learn how to make a 2D Roguelike game with this project.
Over the course of the project will create procedural tile based levels, implement turn based movement, add a hunger system, audio and mobile touch controls. This video series was filmed in Unity 5, but is compatible with Unity 4.6 as well.

Download the assets for free from the Asset Store [here.](https://www.assetstore.unity3d.com/en/?_ga=1.109243084.838993178.1480250241#!/content/29825)

Questions? [Ask in the official forum thread here](http://forum.unity3d.com/threads/2d-roguelike-q-a.297180/?_ga=1.109243084.838993178.1480250241).

##### 1. Setup and Assets

* [ ] [1. Project Introduction]()
* [ ] [2. Player and Enemy Animations]()
* [ ] [3. Creating the Tile Prefabs]()

##### 2. Level Generation

* [ ] [1. Writing the Board Manager]()
* [ ] [2. Writing the Game Manager]()

##### 3. Unit Mechanics

* [ ] [1. Moving Object Script]()
* [ ] [2. Creating Destructible Walls]()
* [ ] [3. Player Animator Controller]()
* [ ] [4. Writing the Player Script]()
* [ ] [5. Writing the Enemy Script]()
* [ ] [6. Enemy Animator Controller]()

##### 4. Architecture and Polish

* [ ] [1. Adding UI & Level Transitions]()
* [ ] [2. Audio and Sound Manager]()
* [ ] [3. Adding Mobile Controls]()

#### 2D UFO Tutorial (9/9)

Create a simple 2D UFO game that teaches you many of the principles of working with Unity and creating 2D games.
In your first foray into Unity development, create a simple 2D UFO game that teaches you many of the principles of working with Game Objects, Components, Prefabs, Physics and Scripting.

Download the assets for free from the asset store [here](https://www.assetstore.unity3d.com/en/content/52143?_ga=1.179519853.838993178.1480250241).

If you have questions, please post in the [Q&A thread on the forums](http://forum.unity3d.com/threads/2d-ufo-tutorial-q-a.381263/?_ga=1.179519853.838993178.1480250241).

##### Introduction & Setup

* [x] [1. Introduction to 2D UFO Project](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/introduction-2d-ufo-project?playlist=25844)
* [x] [2. Setting Up The Play Field](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/setting-play-field?playlist=25844)

##### Movement Basics

* [x] [1. Controlling the Player](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/controlling-player?playlist=25844)
* [x] [2. Adding Collision](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/adding-collision?playlist=25844)
* [x] [3. Following the Player with the Camera](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/following-player-camera?playlist=25844)

##### Collectable Objects

* [x] [1. Creating Collectable Objects](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/creating-collectable-objects?playlist=25844)
* [x] [2. Picking Up Collectables](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/picking-collectables?playlist=25844)
* [x] [3. Counting Collectables and Displaying Score](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/counting-collectables-and-displaying-score?playlist=25844)

##### Creating a Playable Build

* [x] [1. Building our 2D UFO Game](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/building-our-2d-ufo-game?playlist=25844)

#### Adventure Game Tutorial (0/7)

Learn to create the systems used to develop an adventure game in this intermediate level project. Presented in 6 phases, this project will teach you an array of programming techniques to help you better architect your projects.
[Download the Slides as PDF](https://oc.unity3d.com/index.php/s/wCRC5C2WrNdvuBQ?_ga=1.184411759.838993178.1480250241)

Every phase has it's own project but if you would like the completed project and commented scripts
[download it here](https://www.assetstore.unity3d.com/en/?_ga=1.184411759.838993178.1480250241#!/content/76216)

##### Training Day Phases

* [ ] [1. The Player]()
* [ ] [2. The Player (Continued)]()
* [ ] [3. Inventory]()
* [ ] [4. Conditions]()
* [ ] [5. Reactions]()
* [ ] [6. Interactables]()
* [ ] [7. Game State]()

#### Let's Try Assignments (0/12)

In our Let's Try series of tutorials we will explore popular game mechanics in standalone assignments. Instead of creating an entire game, we will focus on a single mechanic and the techniques needed to implement it.

##### Let's Try Assignments

* [ ] [1. Let's Try: Shooting with Raycasts (Video)]()
* [ ] [2. Let's Try: Shooting with Raycasts (article)]()

##### Mini-Projects from the Live Training Archive

* [ ] [7. Couch Wars: Local Multiplayer Basics]()
* [ ] [8. Creating a Basic Platformer Game]()
* [ ] [9. Creating a Breakout Game for Beginners]()
* [ ] [10. Creating a Casual Jewel Mining Game]()

#### Procedural Cave Generation tutorial (0/9)

Learn how to create procedurally generated caverns/dungeons using cellular automata and marching squares.
In this 9 part advanced scripting series created by one of our community members, we learn how to create procedurally generated caverns/dungeons for your games using cellular automata and marching squares.

Author: [Sebastian Lague](https://www.youtube.com/channel/UCmtyQOKKmrMVaKuRXz02jbQ).

There are no asset downloads required for this project but the source code is on github [here](https://github.com/SebLague/Procedural-Cave-Generation).

Questions? [Ask in the official forum thread here](http://forum.unity3d.com/threads/procedural-cave-generation.296986/?_ga=1.112978381.838993178.1480250241).

##### 1. Generating Content

* [ ] [1. Cellular Automata]()
* [ ] [2. Marching Squares]()
* [ ] [3. Creating Meshes]()

##### 2. Walls and Rooms

* [ ] [1. 3D Walls]()
* [ ] [2. Detecting Regions]()
* [ ] [3. Connecting Rooms]()

##### 3. Polish and Passageways

* [ ] [1. Ensuring Connectivity]()
* [ ] [2. Passageways]()
* [ ] [3. Collisions & Textures]()

### Topics

#### Interface & Essentials (6/22)

Everything you need to know to get started using Unity, from basic concepts to extending the interface.

##### Using the Unity Interface

* [x] [1. Interface Overview]()
* [x] [2. The Scene View]()
* [x] [3. The Game View]()
* [x] [4. The Hierarchy and Parent-Child relationships]()
* [x] [5. The Project Panel and Importing]()
* [x] [6. The Inspector]()
* [ ] [7. Build and Player Settings]()
* [ ] [8. Introduction to the Profiler]()

##### Essential Unity Concepts

* [ ] [1. Game Objects and Components]()
* [ ] [2. Prefabs - Concept & Usage]()
* [ ] [3. Tags]()
* [ ] [4. Layers]()

##### Extending the Unity Editor

* [ ] [1. Building a Custom Inspector](https://unity3d.com/learn/tutorials/topics/interface-essentials/building-custom-inspector?playlist=17090)
* [ ] [2. Adding Buttons to a Custom Inspector](https://unity3d.com/learn/tutorials/topics/interface-essentials/adding-buttons-custom-inspector?playlist=17090)
* [ ] [3. The DrawDefaultInspector Function](https://unity3d.com/learn/tutorials/topics/interface-essentials/drawdefaultinspector-function?playlist=17090)
* [ ] [4. Unity Editor Extensions – Menu Items](https://unity3d.com/learn/tutorials/topics/interface-essentials/unity-editor-extensions-menu-items?playlist=17090)
* [ ] [5. Creating Basic Editor Tools](https://unity3d.com/learn/tutorials/topics/scripting/creating-basic-editor-tools?playlist=17117)

##### Live Sessions on Unity Interface and Essentials

* [ ] [1. Editor Basics]()
* [ ] [2. Game Objects]()


#### 2D Game Creation (0/37)

Everything you need to start making 2D games in Unity

##### Unity for 2D

* [ ] [1. 2D Game Development Walkthrough]()
* [ ] [2. 2D Mode]()
* [ ] [3. The Sprite Type]()
* [ ] [4. Sprite Renderer]()
* [ ] [5. The Sprite Editor]()
* [ ] [6. Sorting Layers]()

##### Live Sessions on 2D

* [ ] [1. Introduction to Unity from a 2D perspective]()
* [ ] [2. 2D Character Controllers]()
* [ ] [3. 2D Catch Game - Pt 1]()
* [ ] [4. 2D Catch Game - Pt 2]()
* [ ] [5. 2D Catch Game - Pt 3]()
* [ ] [6. 2D Catch Game - Q&A / AMA]()
* [ ] [7. 2D Scrolling Backgrounds]()
* [ ] [8. Top Down 2D Game Basics]()
* [ ] [10. Making an "Angry Birds" style game - Part 1]()
* [ ] [11. Making an "Angry Birds" style game - Part 2]()
* [ ] [12. 2D Games for Non-Programmers]()
* [ ] [10. 2D Character Controllers](https://unity3d.com/learn/tutorials/topics/2d-game-creation/2d-character-controllers?playlist=17120)

##### Live Session: Making A Flappy Bird Style Game

* [ ] [1. Project Goals]()
* [ ] [2. Adding Sprites]()
* [ ] [3. The Bird Script]()
* [ ] [4. Animating The Bird]()
* [ ] [5. Score and GameOver UI Setup]()
* [ ] [6. Adding The Game Controller]()
* [ ] [7. Scrolling Repeating Backgrounds]()
* [ ] [8. Adding Column Obstacles]()
* [ ] [9. Recycling Obstacles With Object Pooling]()
* [ ] [10. Questions and Answers]()

##### Community Extensions

* [ ] [1. Getting Started with SVG Importer]()

#### Scripting (0/94)

Learn about programming from scratch, then progress to create detailed code for your projects.

##### Beginner Gameplay Scripting

* [x] [1. Scripts as Behaviour Components](engine/topics/scripting/01/01-scripts-as-behaviour-components.md) 3:27
* [x] [2. Variables and Functions](engine/topics/scripting/01/02-variables-and-functions.md) 5:52
* [x] [3. Conventions and Syntax](engine/topics/scripting/01/03-conventions-and-syntax.md) 4:10
* [x] [4. C# vs JS syntax](engine/topics/scripting/01/) 1:54
* [x] [5. IF Statements](engine/topics/scripting/01/) 1:26
* [x] [6. Loops](engine/topics/scripting/01/) 5:33
* [x] [7. Scope and Access Modifiers](engine/topics/scripting/01/) 4:24
* [x] [8. Awake and Start](engine/topics/scripting/01/) 1:37
* [x] [9. Update and FixedUpdate](engine/topics/scripting/01/) 1:43
* [ ] [10. Vector Maths](engine/topics/scripting/01/) 9:03
* [x] [11. Enabling and Disabling Components](engine/topics/scripting/01/) 1:13
* [x] [12. Activating GameObjects](engine/topics/scripting/01/) 2:14
* [x] [13. Translate and Rotate](engine/topics/scripting/01/) 2:57
* [x] [14. Look At](engine/topics/scripting/01/) 1:18
* [x] [15. Linear Interpolation](engine/topics/scripting/01/) no video.
* [x] [16. Destroy](engine/topics/scripting/01/) 2:04
* [x] [17. GetButton and GetKey](engine/topics/scripting/01/) 2:55
* [x] [18. GetAxis](engine/topics/scripting/01/) 3:00
* [x] [19. OnMouseDown](engine/topics/scripting/01/) 1:24
* [x] [20. GetComponent](engine/topics/scripting/01/) 2:52
* [x] [21. Delta Time](engine/topics/scripting/01/) 1:51
* [x] [22. Data Types](engine/topics/scripting/01/) 3:54
* [x] [23. Classes](engine/topics/scripting/01/) 6:00
* [x] [24. Instantiate](engine/topics/scripting/01/) 3:32
* [x] [25. Arrays](engine/topics/scripting/01/) 4:45
* [x] [26. Invoke](engine/topics/scripting/01/) 3:12
* [x] [27. Enumerations](engine/topics/scripting/01/) 4:33
* [x] [28. Switch Statements](engine/topics/scripting/01/) 3:46

##### Intermediate Gameplay Scripting

* [ ] [1. Properties]()
* [ ] [2. Ternary Operator]()
* [ ] [3. Statics]()
* [ ] [4. Method Overloading]()
* [ ] [5. Generics]()
* [ ] [6. Inheritance]()
* [ ] [7. Polymorphism]()
* [ ] [8. Member Hiding]()
* [ ] [9. Overriding]()
* [ ] [10. Interfaces]()
* [ ] [11. Extension Methods]()
* [ ] [12. Namespaces]()
* [ ] [13. Lists and Dictionaries]()
* [ ] [14. Coroutines]()
* [ ] [15. Quaternions]()
* [ ] [16. Delegates]()
* [ ] [17. Attributes]()
* [ ] [18. Events]()

##### Community Posts

* [ ] [1. MonoDevelop's Debugger]()
* [ ] [2. Good Coding Practices in Unity]()
* [ ] [3. Unity Editor Extensions – Menu Items]()
* [ ] [4. Creating Meshes]()

##### Project Architecture

* [ ] [1. AssetBundles and the AssetBundle Manager]()
* [ ] [2. Mastering Unity Project Folder Structure - Version Control Systems]()

##### Getting Started with Unity Development using Visual Studio

* [ ] [1. Installing Tools for Unity Development]()
* [ ] [2. Building your first Unity Game with Visual Studio]()
* [ ] [3. Editing Unity games in Visual Studio]()
* [ ] [4. Debugging Unity games in Visual Studio]()
* [ ] [5. Graphics debugging Unity games in Visual Studio]()
* [ ] [6. Taking Unity games to Universal Windows Platform]()
* [ ] [7. Testing Unity games on Android in Visual Studio]()

##### Live Sessions on Scripting

* [ ] [1. Scripting Primer and Q&A]()
* [ ] [2. Scripting Primer and Q&A - Continued]()
* [ ] [3. Scripting Primer and Q&A - Continued (Again)]()
* [ ] [4. Persistence - Saving and Loading Data]()
* [ ] [5. Object Pooling]()
* [ ] [6. Introduction to Scriptable Objects]()
* [ ] [7. How to communicate between Scripts and GameObjects]()
* [ ] [8. Coding in Unity for the Absolute Beginner]()
* [ ] [9. Sound Effects & Scripting]()
* [ ] [10. Editor Scripting Intro]()
* [ ] [11. Writing Plugins]()
* [ ] [12. Property Drawers & Custom Inspectors]()
* [ ] [13. Events: Creating a simple messaging system]()
* [ ] [14. Using Interfaces to Make a State Machine for AI]()
* [ ] [15. Ability System with Scriptable Objects]()
* [ ] [16. Character Select System with Scriptable Objects]()

##### Live Session: Quiz Game 1

* [ ] [1. Intro and Setup]()
* [ ] [2. Data Classes]()
* [ ] [3. Menu Screen]()
* [ ] [4. Game UI]()
* [ ] [5. Answer Button]()
* [ ] [6. Displaying Questions]()
* [ ] [7. Click To Answer]()
* [ ] [8. Ending The Game and Q&A]()

##### Live Session: Quiz Game 2

* [ ] [1. Intro To Part Two]()
* [ ] [2. High Score with PlayerPrefs]()
* [ ] [3. Serialization and Game Data]()
* [ ] [4. Loading Game Data via JSON]()
* [ ] [5. Loading and Saving via Editor Script]()
* [ ] [6. Game Data Editor GUI]()
* [ ] [7. Question and Answer]()

#### Best Practices (0/12)

Learn production tested best practices from our enterprise support engineers.

##### Assets, Resources and AssetBundles

* [ ] [1. A guide to AssetBundles and Resources]()
* [ ] [2. Assets, Objects and serialization]()
* [ ] [3. The Resources folder]()
* [ ] [4. AssetBundle fundamentals]()
* [ ] [5. AssetBundle usage patterns]()

##### Optimizing Unity UI

* [ ] [1. A guide to optimizing Unity UI]()
* [ ] [2. Fundamentals of Unity UI]()
* [ ] [3. Unity UI Profiling Tools]()
* [ ] [4. Fill-rate, Canvases and input]()
* [ ] [5. Optimizing UI Controls]()
* [ ] [6. Other UI Optimization Techniques and Tips]()

#### Graphics (62)

Everything for Lighting and Rendering in Unity

##### Introduction to Lighting and Rendering

* [ ] [1. Introduction to Lighting and Rendering]()
* [ ] [2. Choosing a Lighting Technique]()
* [ ] [3. The Precompute Process]()
* [ ] [4. Choosing a Rendering Path]()
* [ ] [5. Choosing a Color Space]()
* [ ] [6. High Dynamic Range (HDR)]()
* [ ] [7. Reflections]()
* [ ] [8. Ambient Lighting]()
* [ ] [9. Light Types]()
* [ ] [10. Emissive Materials]()
* [ ] [11. Light Probes]()

##### Precomputed Realtime GI

* [ ] [1. Introduction to Precomputed Realtime GI]()
* [ ] [2. Realtime Resolution]()
* [ ] [3. Understanding Charts]()
* [ ] [4. Starting the precompute process]()
* [ ] [5. Probe lighting]()
* [ ] [6. Unwrapping and Chart reduction]()
* [ ] [7. Understanding Clusters]()
* [ ] [8. Fine tuning with Lightmap Parameters]()
* [ ] [9. Summary - Precomputed Realtime GI]()

##### Rendering and Shading

* [ ] [1. Lighting Overview]()
* [ ] [2. Lights]()
* [ ] [3. Materials]()
* [ ] [4. The Standard Shader]()
* [ ] [5. Textures]()
* [ ] [6. Using Skyboxes]()
* [ ] [7. A Gentle Introduction to Shaders]()
* [ ] [8. Using detail textures for extra realism close-up]()
* [ ] [9. Frame Debugger]()

##### Cameras and Effects

* [ ] [1. Cameras]()
* [ ] [2. Image Effects: Overview]()

##### Geometry in Unity

* [ ] [1. Meshes]()
* [ ] [2. Mesh Renderers and Mesh Filters]()

##### Live Sessions on Graphics

* [ ] [1. Using Cameras]()
* [ ] [2. Using Lights]()
* [ ] [3. Fun with Lasers!]()
* [ ] [4. The Particle System]()
* [ ] [5. Cinematic Explosions - PIT]()
* [ ] [6. Cinematic Composition - PIT]()
* [ ] [7. Image Effects: Overview]()
* [ ] [8. Fun with Explosions!]()
* [ ] [9. Exploring the Blacksmith Environment]()

##### Unite Presentations

* [ ] [1. Turning it up to 11: Making Unity 5 look Awesome!]()

##### Substance (by Allegorithmic)

* [ ] [1. Substance - Introduction]()
* [ ] [2. Substance - Understanding PBR]()
* [ ] [3. Substance - Working with PBR in Unity]()
* [ ] [4. Substance - Using Substance materials in Unity]()
* [ ] [5. Substance - Optimization for Substance materials]()
* [ ] [6. Substance - Creating rock shapes]()
* [ ] [7. Substance - Creating rock material, Pt 1]()
* [ ] [8. Substance - Creating rock material, Pt 2]()
* [ ] [9. Substance - Creating the dirt ground material]()
* [ ] [10. Substance - Creating the rock ground material, Pt 1]()
* [ ] [11. Substance - Creating the rock ground material, Pt 2]()
* [ ] [12. Substance - Publishing the Substance]()
* [ ] [13. Substance - Creating a blocking scene]()
* [ ] [14. Substance - Creating the ground model]()
* [ ] [15. Substance - Modelling the rock assets]()
* [ ] [16. Substance - Texturing the upper body]()
* [ ] [17. Substance - Exporting textures from Substance Painter]()
* [ ] [18. Substance - Creating a scene in Unity, Pt 1]()
* [ ] [19. Substance - Creating a scene in Unity, Pt 2]()

#### Physics (24)

Create some mechanical mayhem as you learn about Unity’s physics options.

##### 3D Physics

* [ ] [1. Colliders]()
* [ ] [2. Colliders as Triggers]()
* [ ] [3. Rigidbodies]()
* [ ] [4. Adding Physics Forces]()
* [ ] [5. Adding Physics Torque]()
* [ ] [6. Physics Materials]()
* [ ] [7. Physics Joints]()
* [ ] [8. Detecting Collisions with OnCollisionEnter]()
* [ ] [9. Raycasting]()

##### 2D Physics

* [ ] [1. 2D Physics Overview](https://unity3d.com/learn/tutorials/topics/2d-game-creation/2d-physics-overview?playlist=17093)
* [ ] [2. Rigidbody 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/rigidbody-2d?playlist=17093)
* [ ] [3. Collider 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/collider-2d?playlist=17093)
* [ ] [4. Bouncing & Sliding in 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/bouncing-sliding-2d?playlist=17093)
* [ ] [5. Distance Joint 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/distance-joint-2d?playlist=17120)
* [ ] [6. Hinge Joint 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/hinge-joint-2d?playlist=17093)
* [ ] [7. Spring Joint 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/spring-joint-2d?playlist=17120)
* [ ] [8. Area Effector 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/area-effector-2d?playlist=17120)
* [ ] [9. Point Effector 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/point-effector-2d?playlist=17120)
* [ ] [10. Surface Effector 2D and Platform Effector 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/surface-effector-2d-and-platform-effector-2d?playlist=17120)

##### Assignments

* [ ] [1. Bouncing Ball](https://unity3d.com/learn/tutorials/topics/physics/bouncing-ball?playlist=17120)
* [ ] [2. Brick Shooter](https://unity3d.com/learn/tutorials/topics/physics/brick-shooter?playlist=17120)

##### Live Training On Physics

* [ ] [1. Creating a Hover Car with Physics](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/hover-car-physics?playlist=17120)
* [ ] [2. 2D Physics: Fun with Effectors](https://unity3d.com/learn/tutorials/topics/2d-game-creation/2d-physics-fun-effectors?playlist=17120)

##### Articles

* [ ] [1. Physics Best Practices]()


#### Audio (12)

Everything for Game Audio and Sound design in Unity

##### Audio Setup

* [ ] [1. Audio Listeners & Sources]()

##### Audio Mixing

* [ ] [1. Audio Mixer and Audio Mixer Groups]()
* [ ] [2. Audio Effects]()
* [ ] [3. Send and Receive Audio Effects]()
* [ ] [4. Duck Volume Audio Effect]()
* [ ] [5. Audio Mixer Snapshots]()
* [ ] [6. Exposed AudioMixer Parameters]()

##### Live Trainings on Audio

* [ ] [1. Sound Effects & Scripting]()
* [ ] [2. Adding Music to Your Game]()
* [ ] [3. Sound Effects in Unity 5]()

##### Upgrading Audio to Unity 5

* [ ] [1. Upgrading Unity 4 Audio Part 1: Mixers and Effects]()
* [ ] [2. Upgrading Unity 4 Audio Part 2: Snapshots and Exposed Parameters]()


#### Animation (18)

Get things moving! all you need to start animating in Unity.

##### Animating

* [ ] [1. The Animation View]()
* [ ] [2. Animation Properties]()
* [ ] [3. Animation Curves and Events]()
* [ ] [4. Animation Asset API]()

##### Controlling Animation

* [ ] [1. The Animator Component]()
* [ ] [2. The Animator Controller]()
* [ ] [3. Animator Controller Layers]()
* [ ] [4. Animator Scripting]()
* [ ] [5. Blend Trees]()
* [ ] [6. Animator Sub-state Machine hierarchies]()

##### Character Animation

* [ ] [1. Humanoid Avatars]()
* [ ] [2. Authoring Root Motion]()
* [ ] [3. Avatar Masks]()

##### Live Sessions on Animation

* [ ] [1. Animate Anything with Mecanim]()
* [ ] [2. Character Animation Setup]()

##### Community Posts on Animation

* [ ] [1. Direct Blend Trees]()
* [ ] [2. Apex Path and Mecanim - Article]()

#### User Interface (UI) (41)

Learn to use Unity's tools for designing user interfaces (UI).

##### UI Components (11)

* [ ] [UI Canvas](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-canvas?playlist=17111)
* [ ] [UI RectTransform](https://unity3d.com/learn/tutorials/modules/beginner/ui/rect-transform?playlist=17111)
* [ ] [UI Button](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-button?playlist=17111)
* [ ] [UI Image](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-image?playlist=17111)
* [ ] [UI Text](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-text?playlist=17111)
* [ ] [UI Events and Event Triggers](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-events-and-event-triggers?playlist=17111)
* [ ] [UI Slider](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-slider?playlist=17111)
* [ ] [UI Transitions](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-transitions?playlist=17111)
* [ ] [UI ScrollRect](https://unity3d.com/learn/tutorials/modules/beginner/ui/ui-scroll-rect?playlist=17111)
* [ ] [UI Scrollbar](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-scrollbar?playlist=17111)
* [ ] [UI Mask](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-mask?playlist=17111)

##### Live Sessions On UI (10)

* [ ] [The new UI](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/the-new-ui?playlist=17111)
* [ ] [Using the UI tools](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/using-the-ui-tools?playlist=17111)
* [ ] [Game jam menu template](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/game-jam-template?playlist=17111)
* [ ] [Creating a scene selection menu](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/creating-a-scene-menu?playlist=17111)
* [ ] [Panes, panels and windows](https://unity3d.com/learn/tutorials/modules/intermediate/live-training-archive/panels-panes-windows?playlist=17111)
* [ ] [Making a generic modal window](https://unity3d.com/learn/tutorials/modules/intermediate/live-training-archive/modal-window?playlist=17111)
* [ ] [Making a generic modal window (Pt 2)](https://unity3d.com/learn/tutorials/modules/intermediate/live-training-archive/modal-window-pt2?playlist=17111)
* [ ] [Making a generic modal window (Pt 3)](https://unity3d.com/learn/tutorials/modules/intermediate/live-training-archive/modal-window-pt3?playlist=17111)
* [ ] [Creating A Main Menu](https://unity3d.com/learn/tutorials/topics/user-interface-ui/creating-main-menu?playlist=17111)
* [ ] [Polishing Your Game Menu](https://unity3d.com/learn/tutorials/topics/user-interface-ui/polishing-your-game-menu?playlist=17111)

##### Creating a Tic-Tac-Toe game using only the UI (11)

* [ ] [Introduction and setting-up the project](https://unity3d.com/learn/tutorials/tic-tac-toe/introduction-and-setting-project?playlist=17111)
* [ ] [Creating the game board](https://unity3d.com/learn/tutorials/tic-tac-toe/creating-game-board?playlist=17111)
* [ ] [Interaction with the game board](https://unity3d.com/learn/tutorials/tic-tac-toe/interaction-game-board?playlist=17111)
* [ ] [Foundation game play](https://unity3d.com/learn/tutorials/tic-tac-toe/foundation-game-play?playlist=17111)
* [ ] [Controlling the game](https://unity3d.com/learn/tutorials/tic-tac-toe/game-control?playlist=17111)
* [ ] [Win conditions and taking turns](https://unity3d.com/learn/tutorials/tic-tac-toe/win-conditions-and-taking-turns?playlist=17111)
* [ ] [Game Over text](https://unity3d.com/learn/tutorials/tic-tac-toe/game-over-text?playlist=17111)
* [ ] [Ending in a draw](https://unity3d.com/learn/tutorials/tic-tac-toe/draw?playlist=17111)
* [ ] [Restarting the game](https://unity3d.com/learn/tutorials/tic-tac-toe/restarting-game?playlist=17111)
* [ ] [Whose turn is it?](https://unity3d.com/learn/tutorials/tic-tac-toe/whose-turn-it?playlist=17111)
* [ ] [Choosing the starting side and starting the game](https://unity3d.com/learn/tutorials/tic-tac-toe/choosing-starting-player-side-and-starting-game?playlist=17111)


##### Live Training: Shop UI with Runtime Scroll Lists(9)

* [ ] [Intro and Setup](https://unity3d.com/learn/tutorials/topics/user-interface-ui/intro-and-setup?playlist=17111)
* [ ] [Scroll View](https://unity3d.com/learn/tutorials/topics/user-interface-ui/scroll-view?playlist=17111)
* [ ] [Adding Buttons](https://unity3d.com/learn/tutorials/topics/user-interface-ui/adding-buttons?playlist=17111)
* [ ] [Button Prefab](https://unity3d.com/learn/tutorials/topics/user-interface-ui/button-prefab?playlist=17111)
* [ ] [ShopScrollList Script](https://unity3d.com/learn/tutorials/topics/user-interface-ui/shopscrolllist-script?playlist=17111)
* [ ] [Adding Buttons Via Script](https://unity3d.com/learn/tutorials/topics/user-interface-ui/adding-buttons-script?playlist=17111)
* [ ] [Creating Items and Testing](https://unity3d.com/learn/tutorials/topics/user-interface-ui/creating-items-and-testing?playlist=17111)
* [ ] [Adding Interactivity](https://unity3d.com/learn/tutorials/topics/user-interface-ui/adding-interactivity?playlist=17111)
* [ ] [Creating Second Shop, Q&A](https://unity3d.com/learn/tutorials/topics/user-interface-ui/creating-second-shop-qa?playlist=17111)

#### Mobile & Touch (0/6)

Information for developers creating content for mobile and tablet devices.

##### Getting Input

* [ ] [1. Multi Touch Input]()
* [ ] [2. Accelerometer Input]()
* [ ] [3. Pinch to Zoom]()

##### Building

* [ ] [1. Building your Unity game to an iOS device for testing]()
* [ ] [2. Building your Unity game to an Android device for testing]()

##### Mobile optimization

* [ ] [1. Unite Europe 2016 - Optimizing Mobile Applications]()

#### Navigation (0/7)

Find out about Unity's 'Nav Mesh' pathfinding system.

##### Navigation Basics

* [ ] [1. Navigation Overview]()
* [ ] [2. NavMesh Baking]()
* [ ] [3. The NavMesh Agent]()
* [ ] [4. Off-mesh Links]()
* [ ] [5. NavMesh Obstacles]()

##### Live Sessions On Navigation

* [ ] [1. Nav Meshes]()
* [ ] [2. Creating a Click to Move Game]()

#### Tips (0/19)

##### Tips

* [ ] [1. Snapping]()
* [ ] [2. Undock the Preview window]()
* [ ] [3. Forum API Search]()
* [ ] [4. Positioning Cameras easily]()
* [ ] [5. Access the Asset Store from the Project]()
* [ ] [6. Play Mode Editor Tint]()
* [ ] [7. Documentation Shortcut]()
* [ ] [8. Show public variables as Sliders with [Range(min, max)]]()
* [ ] [9. Find objects in the Scene by Component name]()
* [ ] [10. Math Calculations in Number Fields]()
* [ ] [11. Use custom icons for empty game objects in Scene views]()
* [ ] [12. How to copy and restore changes made in play mode]()
* [ ] [13. Show private variables in the Inspector]()
* [ ] [14. Create new scripts fast with Add Component shortcut]()

##### Live Training Sessions

* [ ] [1. Unity Tips & Tricks]()
* [ ] [2. Unity Tips & Tricks 2]()
* [ ] [3. Unity Tips & Tricks 3]()
* [ ] [4. Unity Tips & Tricks 4]()
* [ ] [5. Unity Tips and Tricks Grab Bag]()

#### Ads & Analytics (0/9)

Learn how to use analytics to segment and understand your audience and get actionable insights into your players' behavior.

##### Analytics Basics

* [ ] [1. Intro to Custom Events]()
* [ ] [2. Introduction to Funnels]()
* [ ] [3. Introduction to Custom Segments]()
* [ ] [4. Glossary of Metrics]()

##### Unity IAP

* [ ] [1. Integrating Unity IAP In Your Game]()

##### Unite Presentations

* [ ] [1. Introduction to Unity Analytics]()

##### Unity Ads 2.0

* [ ] [1. Unity Ads 2.0 Integration Tutorial - Android]()
* [ ] [2. Unity Ads 2.0 Integration Tutorial - Objective C for iOS]()
* [ ] [3. Unity Ads 2.0 Integration Tutorial - Swift for iOS]()

#### Virtual Reality (0/8)

Get started developing for VR platforms with these articles and the VR Samples project.

##### Introduction to VR

* [ ] [1. VR Overview]()
* [ ] [2. Getting Started with VR Development]()
* [ ] [3. Interaction in VR]()
* [ ] [4. User Interfaces for VR]()
* [ ] [5. Movement in VR]()
* [ ] [6. Deploying Your VR Project]()
* [ ] [7. Optimisation for VR in Unity]()
* [ ] [8. The VR Reading List]()

#### Performance Optimization (0/4)

Learn how to diagnose common performance problems and optimize your projects.

##### Diagnosing performance problems

* [ ] [1. The Profiler window]()
* [ ] [2. Diagnosing performance problems using the Profiler window]()

##### Fixing performance problems

* [ ] [1. Optimizing garbage collection in Unity games]()
* [ ] [2. Optimizing graphics rendering in Unity games]()

#### Multiplayer Networking (0/21)

Learn to create Multiplayer Networked games with these Lessons and Assignments.

##### Creating a simple multiplayer example

* [ ] [1. Introduction to a Simple Multiplayer Example]()
* [ ] [2. The Network Manager]()
* [ ] [3. Setting up the Player prefab]()
* [ ] [4. Registering the Player prefab]()
* [ ] [5. Creating Player Movement (Single Player)]()
* [ ] [6. Testing Player Movement Online]()
* [ ] [7. Networking Player Movement]()
* [ ] [8. Testing Multiplayer Movement]()
* [ ] [9. Identifying the Local Player]()
* [ ] [10. Shooting (Single Player)]()
* [ ] [11. Adding Multiplayer shooting]()
* [ ] [12. Player Health (Single Player)]()
* [ ] [13. Networking Player Health]()
* [ ] [14. Death and Respawning]()
* [ ] [15. Handling Non-Player objects]()
* [ ] [16. Destroying Enemies]()
* [ ] [17. Spawning and Respawning]()
* [ ] [18. Simple Game Summary]()

##### Live Session: Merry Fragmas 3.0

* [ ] [1. Merry Fragmas 3.0: Multiplayer FPS Foundation]()
* [ ] [2. Merry Fragmas 3.0: UI, Graphics, and Animations]()
* [ ] [3. Merry Fragmas 3.0: Lobby, Player Data, and Deathmatch Rules]()


## Services & Production

### Topics

#### Cloud Build (4)

Learn how use Unity Cloud Build to optimize your build process.

##### Lessons

* [ ] [1. Introduction to Unity Cloud Build]()
* [ ] [2. Creating Your First Source Control Repository]()
* [ ] [3. Your First Cloud Build Project]()

##### Appendix

* [ ] [1. Appendix: Build Process Fundamentals]()

#### The Asset Store (14)

Lessons can be used as a reference and watched without following steps.

##### Asset Store Basics

* [ ] [1. Using the Asset Store]()
* [ ] [2. Access the Asset Store from the Project]()

##### Community Posts

* [ ] [1. ProBuilder Overview]()
* [ ] [2. Camera Perspective Editor]()
* [ ] [3. Introduction to iCanScript]()
* [ ] [4. Getting Started with SVG Importer]()
* [ ] [5. Extreme AI - Create a personality, attach it to a character]()
* [ ] [6. xARM Overview]()

##### Community Playlist - Apex Game Tools

* [ ] [1. Getting Started with Apex Path]()
* [ ] [2. Apex Path and Mecanim - Article]()
* [ ] [3. Apex Path and Mecanim - Video]()
* [ ] [4. Upgrading to Apex Path 2.0]()
* [ ] [5. Apex Steer Introduction]()
* [ ] [6. Apex Utility AI Introduction]()

#### Developer Advice (7)

Advice on finishing, releasing and marketing your games.

##### Your First Game

* [ ] [1. How to Start Your Game Development]()
* [ ] [2. Setting (and Keeping) Production Goals]()
* [ ] [3. How to Scope Small and Start Right]()
* [ ] [4. How to Market Your Game]()

##### PR & Marketing

* [ ] [1. Zero budget game marketing]()
* [ ] [2. Marketing Principles for Indies]()
* [ ] [3. How to Market Your Game]()

#### Game Performance Reporting (1)

Learn how to find out how your game is running on various devices and OSs.

##### Game Performance Reporting

* [ ] [1. Getting Started with Game Performance Reporting]()

#### Production (2)

Learn how to take your game from planning, development to release.

##### Marketing and PR

* [ ] [1. Zero budget game marketing]()
* [ ] [2. Marketing Principles for Indies]()


Orgainzied By Hexcola
Start From 2017.01.05