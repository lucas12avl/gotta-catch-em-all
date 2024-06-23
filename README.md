```markdown
# Gotta Catch 'Em All: Pokémon Web Application

Welcome to the **Gotta Catch 'Em All** Pokémon web application! This project is a simplified version of Pokémon Red's Route 1, where the player can walk around, enter tall grass to find Pokémon, catch them, view them in the Pokédex, and save their game progress. The project is divided into two main parts: the frontend, developed with Vue.js, and the backend, built with Node.js.

## Table of Contents

- [Objectives](#objectives)
- [Features](#features)
- [Project Structure](#project-structure)
  - [Frontend (Vue.js)](#frontend-vuejs)
  - [Backend (Node.js)](#backend-nodejs)
- [Installation](#installation)
  - [Frontend](#frontend)
  - [Backend](#backend)
- [Usage](#usage)
- [Endpoints](#endpoints)
- [Components](#components)
  - [Middleware Components](#middleware-components)
- [Screenshots and GIFs](#screenshots-and-gifs)
  - [Gameplay Overview](#gameplay-overview)
  - [Initial Setup](#initial-setup)
  - [Wild Encounter](#wild-encounter)
  - [Capturing Pokémon](#capturing-pokémon)
  - [Pokédex View](#pokédex-view)
- [Authors](#authors)

## Objectives

The main objectives of this project are:
- Reinforce and demonstrate Vue.js concepts through practical application.
- Implement a fully functional backend using Node.js and Express.js.
- Develop a seamless client-server communication using HTTP requests.
- Create an engaging user experience that mimics the classic Pokémon gameplay.

## Features

- **Explore Route 1**: Move around the map and explore the environment.
- **Wild Encounters**: Enter tall grass to randomly encounter wild Pokémon.
- **Catch Pokémon**: Use Pokéballs to catch encountered Pokémon.
- **Pokédex**: View all the Pokémon you have caught in your personal Pokédex.
- **Save Progress**: Save your game progress and continue later.

## Project Structure

### Frontend (Vue.js)

The frontend part of the project is organized as follows:

- `package.json`: Contains the project dependencies and scripts for building and running the application.
- `/src`: Contains all the source files for the Vue.js application.
  - `App.vue`: The main Vue component that serves as the entry point.
  - `/components`: Directory containing custom Vue components used in the application.
  - `/base_components`: Contains reusable base components.
  - `globals.js`: Contains global variables and configurations used throughout the Vue components.

More specific information at gotta_catch_em_all_frontend.pdf

### Backend (Node.js)

The backend part of the project is structured as follows:

- `package.json`: Contains the project dependencies and scripts for starting the server.
- `app.js`: The main server file that initializes and starts the Express server.
- `/public`: Contains the compiled and static files of the Vue frontend.
- `/routes`: Contains route definitions and logic for handling various API requests.

More specific information at gotta_catch_em_all_backend.pdf

## Installation

Follow these steps to set up the project locally:

### Frontend

1. Clone the repository and navigate to the frontend directory:
   ```bash
   git clone https://github.com/lucas12avl/gotta-catch-em-all.git
   cd gotta-catch-em-all/frontend
   ```
2. Install the dependencies:
   ```bash
   npm install
   ```
3. Start the development server:
   ```bash
   npm run dev
   ```

### Backend

1. Navigate to the backend directory:
   ```bash
   cd gotta-catch-em-all/backend
   ```
2. Install the dependencies:
   ```bash
   npm install
   ```
3. Start the server:
   ```bash
   node app.js
   ```

## Usage

To use the application, follow these steps:

1. Ensure both the frontend and backend servers are running.
2. Open your browser and navigate to `http://localhost:5173/` to access the application.
3. Use the on-screen controls to move your character around Route 1.
4. Enter tall grass to potentially encounter wild Pokémon.
5. Attempt to catch Pokémon using Pokéballs.
6. View your captured Pokémon in the Pokédex.
7. Save your progress to continue your adventure later.
**Hint:** 
Have you captured all 2 different pokmons? 
Then, go to the sign at the bottom of the map and press the A button to reveal an Easter egg! 
(maybe another different pokemon is waiting for you)

## Endpoints

The backend provides the following API endpoints:

- `GET /initial_info`: Returns the initial game information including player position, Pokémon data, and game settings.
- `POST /enter_grass`: Triggered when the player enters tall grass, potentially resulting in a wild Pokémon encounter.
- `POST /leave_grass`: Triggered when the player leaves tall grass, ending any ongoing wild encounter.
- `POST /capture`: Used to attempt capturing a wild Pokémon.
- `POST /save`: Saves the current game state including player progress and captured Pokémon.

## Components

### Middleware Components

The middleware components are essential parts of the game logic, handling various aspects of the gameplay:

- **ControlsFrame**: Manages button press and release events for character movement and interactions.
- **MapMiddleware**: Handles the display of the game map and character navigation.
- **PlayerMiddleware**: Manages the player sprite, including direction and movement.
- **WildEncounterMiddleware**: Manages wild Pokémon encounters when the player enters tall grass.
- **MenuMiddleware**: Displays the game menu and handles actions like saving the game.

## Screenshots and GIFs

### Gameplay Overview
![Gameplay Overview](./project%20images/gameplay.gif)

### Initial Setup
![Initial Setup](./project%20images/initialSetup.png)

### Wild Encounter
![Wild Encounter](./project%20images/capturingGameplay.gif)

### Capturing Pokémon
<img src="./project%20images/wildEncounter.png" alt="wild encounter" width="300"/>
<img src="./project%20images/pidgeyCaught.png" alt="wild encounter" width="300"/>

### Pokédex View
![Pokédex View](./project%20images/pokedex.gif)
*A screenshot showing the Pokédex with captured and seen Pokémon.*

## Authors
Lucas Dalmau
