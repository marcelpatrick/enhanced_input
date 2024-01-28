# enhanced_input

##Preparation
- create a c++ character class
- create a BP based on this class
- - Include a skeletal mesh

##Development
- in the c++ class
    - header file
        - instantiate a mapping context var and expose it to the BP
        - instantiate input action vars (one for each input: move, jump etc) and expose it to the BP
        - declare a function for each input action (eg: move) passing _by Reference_ an **FInputActionValue** as a param.
        - - _Passing a param by reference **&** allows the function to update the value of the original variable outside of the function and not only the value of the param inside the function_
    - cpp
        ### Add Input mapping Context:
        ####Definitions:
        - Mapping Contexts allow developers to create different input configurations for different situations or game states. For example, the input mappings for a character might differ when they are in a combat state versus when they are in a menu or driving a vehicle
        - Player Subsystems:
          - In Unreal Engine, subsystems are modular components that provide specific functionality or services within the engine. They are designed to be reusable and self-contained.
          - Types of Subsystems: There are various types of subsystems, including Engine Subsystems (global across the game), GameInstance Subsystems (tied to the game instance), and Player Subsystems (tied to a specific player).
          - A Player Subsystem is a type of subsystem that is specifically tied to an individual player. In multiplayer games or games that handle multiple player profiles, each player would have their own instance of a Player Subsystem.
          - Use Cases: This allows for functionalities that are unique to each player, such as specific settings, states, or input handling, to be managed independently for each player._
          ####Implementation:
          - On Begin Play
            - Declare a player controller var by taking the controller and casting it to APlayerController type
            - Use the player controller var to get the local player 
              - Use the local player to get the subsystem and Declare a Subsystem var
               - Use use the subsystem var to add a mapping context
                 
### BIND functionality to input
- cpp
- SetupPlayerInputComponent function takes a PlayerInputComponent as input
- - Take **PlayerInputComponent** and Cast it to **EnhancedInputComponent**
  - Use **EnhancedInputComponent** to Bind Action calling the action function (Move)
 
### Move function
- cpp
- The move function takes a **FInputActionValue** as input
- Instantiate a FVector2D variable
- Find out which way is forward by getting the Yaw rotation
- Get X Axis
- Get Y Axis
- Add movement input
- - Get the forward direction in the **Unreal Engine** world -> _in Unreal Engine Forward = X_
  - **Invert Axis**: Map the Unreal Engine forward to the **Game World** -> _according to game standards, player input devices are mapped with Forward = Y_

### Set Context Mapping and Input assets in the Engine
- In Unreal Engine
- Create a context mapping asset
- - add a mapping
  - - map to a key
    - - add a modifier
      - - use the Swizzle input axis values modifier to map the W key to the Y axis in a 2D enviornment
       
## Link to the BP
- in the Character BP, assign the Context Mapping and the Input assets to the character.
    
