# enhanced_input

## Preparation
- create a c++ character class
- create a BP based on this class
- - Include a skeletal mesh

## Development
- in the c++ class
    - header file
        - instantiate a mapping context var and expose it to the BP
        - instantiate input action vars (one for each input: move, jump etc) and expose it to the BP
        - declare a function for each input action (eg: move) passing _by Reference_ an **FInputActionValue** as a param.
        - - _Passing a param by reference **&** allows the function to update the value of the original variable outside of the function and not only the value of the param inside the function_
    - cpp
        ### Add Input mapping Context:
        ```
        - Mapping Contexts allow developers to create different input configurations for different situations or game states. For example, the input mappings for a character might differ when they are in a combat state versus when they are in a menu or driving a vehicle
        - Player Subsystems:
          - In Unreal Engine, subsystems are modular components that provide specific functionality or services within the engine. They are designed to be reusable and self-contained.
          - Types of Subsystems: There are various types of subsystems, including Engine Subsystems (global across the game), GameInstance Subsystems (tied to the game instance), and Player Subsystems (tied to a specific player).
          - A Player Subsystem is a type of subsystem that is specifically tied to an individual player. In multiplayer games or games that handle multiple player profiles, each player would have their own instance of a Player Subsystem.
          - Use Cases: This allows for functionalities that are unique to each player, such as specific settings, states, or input handling, to be managed independently for each player._


  #### Implementation:
  - On Begin Play
    - Declare a player controller var by taking the controller and casting it to APlayerController type
    - Use the player controller var to get the local player 
      - Declare a **UEnhancedInputLocalPlayerSubsystem** var by getting the subsystem, converting it to a **UEnhancedInputLocalPlayerSubsystem** var and passing the local player var as a param
       - Use use the subsystem var to add a mapping context passing my mapping context var as a param
                 
### BIND functionality to input
- cpp
- in the SetupPlayerInputComponent function:
- - Take **PlayerInputComponent** and Cast it to **EnhancedInputComponent**
  - Use **EnhancedInputComponent** to Bind Action passing my input action variable calling the action function (Move) by its address
  - ```
    The ampersand & here is used to get the address of the Move member function in the AMyCharacter_2 class.
    ```
 
### Move function
- cpp
- Instantiate a FVector2D variable by using the input action to get a vector 2d
- Find out which way is forward by getting the control rotation and then the Yaw rotation

- - Get the forward direction in the **Unreal Engine** world -> _in Unreal Engine Forward is the X axis_
  - - Get X Axis and assign it to a foward direction FVector var - use **FRotationMatrix** and **GetUnitAxis**
  - - Get Y Axis and assing it to a right direction FVector var - use **FRotationMatrix** and **GetUnitAxis**
  - - Add movement
    - - Add movement input inverting the axis. Pass in the forwar direction to the Y Vector2d axis and the right direction to the X Vector2d axis
  - - - **Invert Axis**: Map the Unreal Engine forward to the **Game World** -> _according to game standards, player input devices are mapped with Forward = Y_

### Set Context Mapping and Input assets in the Engine
- In Unreal Engine
- Create a context mapping asset
- - add a mapping
  - - map to a key
    - - add a modifier
      - - use the Swizzle input axis values modifier to map the W key to the Y axis in a 2D enviornment
       
## Link to the BP

### Input Action
- Create an input action and make it a value type vector 2d

### Input Mapping Context
- Create a mapping context
- Add mapping
- Select my input action in the mapping dropdown
- Select the key for Forward motion, add a modifier and select "swizzle"
- Select the key for Backward motion, add modifier and select "swizzle", add another modifier and select negate (the oposite of forward)
```
Swizzle inverts the X and Y axis. In UE the X axis is the vertical axis (forward, backward) in game convention it is the Y axis that controls that
```
- Select the key for Right motion
- Select the key for Left motion, add modifier and select negate (the oposite of right)
  
- in the Character BP, assign the Context Mapping and the Input assets to the character.
    
