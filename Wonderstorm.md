As one of the founding members of the team, I've led the effort of building many of our core multiplayer gameplay systems from the ground-up across our Unreal Engine-powered game server, game client, and backend live services platform (built in collaboration with our partners at [Pragma Engine](https://pragma.gg)). In addition to working across nearly every system in the game, I've mentored junior engineering team members in our Unreal C++ codebase, as well as provided support and video/Confluence documentation for designers and artists working with our gameplay systems via Unreal Blueprints.

My role as a Principal Gameplay Engineer requires that I work hand-in-hand with our art, design, and backend platform teams to bring clarity to feature requests by defining requirements, translating them into robust and extensible gameplay systems, and iterating with design stakeholders on the data structures and interfaces. The job doesn't end once the system in place - by engaging with the designers and other engineers on our on our team to solicit feedback, I relentlessly pursue improved workflows, reduce iteration times, and optimize our game's code using Unreal Engine's suite of profiling tools.

Here's a summary of the areas of the game that I have worked on during my time at Wonderstorm:

### **Characters**
- Developed character pipeline that allowed the art and design team to build 11 playable heroes, over 70 unique enemy NPCs, and 6 set-piece world bosses
- Designed a "phase" system for bosses, leveraging a finite state machine that designers could use to control the boss's animation set, available abilities, and AI behavior, utilizing our custom syntax for expressing state change transition conditions

### **Gameplay Ability System**
- Integrated [Unreal's Gameplay Ability System](https://docs.unrealengine.com/en-US/gameplay-ability-system-for-unreal-engine/) plugin into our project
- Automated tedious aspects of the system (such as defining gameplay tags, abilities, effects, cues, and attributes) using our custom data language and code generation techniques
- Created a common gameplay ability finite state machine (FSM) that allowed designers to script transitions between ability states and other combo abilities
  - Designers created data for over 600 unique abilities using this single C++ gameplay ability class over the course of the project
- Created a system of easily tunable ability "actions" for designers to leverage in their abilities that fully handled prediction, replication, and rollback for common gameplay features such as targeting, physics, movement, visual/sound cues, status effects, etc.
- Created a system of "gameplay events" that designers could use to script additional behavior for passive bonuses granted by equipping items and character progression
- Exposed key aspects of Gameplay Ability System to Blueprint for designers

### **Character Movement**
- Developed a deep understanding of Unreal's [`UCharacterMovementComponent`](https://docs.unrealengine.com/en-US/API/Runtime/Engine/GameFramework/UCharacterMovementComponent/) source code, and how to properly predict and replicate client movement
- Authored custom [`FRootMotionSources`](https://docs.unrealengine.com/en-US/API/Runtime/Engine/GameFramework/FRootMotionSource/) for our project's unique movement abilities based on design's specifications, including tunable combat knockbacks, procedural player-controlled sliding based on the slope/normal of surfaces, and a grappling hook for flying throughout levels
- Added support to the UE character movement component for instant rotations towards combat targets or the player's input direction, to make combat and targeting feel snappier
- Designed a "movement modifier" system to allow for prioritizing stacked additive or override settings to the UE character movement component, allowing designers to author predictive/replicated modifications of move speed, direction, rotation mode, and other movement settings during abilities

### **Animation Blueprints/State Machines**
- Wrote C++ [`UAnimInstance`](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Animation/UAnimInstance/) logic for synchronizing character animations with networked movement state, including…
  - Run/walk locomotion
  - Movement states for falling, flying, and sliding
  - Foot and head/look-at IK
  - Strafe movement (8 way/2D blendspaces) for twinstick control
  - Pose blending (upper body only animations) for specific ability states
- Developed a system to enable animators to drag-and-drop character animations for shared abilities and locomotion "motion sets" into a data asset that was utilized by a single, shared parent animation blueprint graph for every character in our game (nearly 100 characters, many with unique skeletons)
  - With this tech, we were able to avoid copy/pasting animation graph logic into several different animation blueprints, and maintain a single, shared animation state machine for all of our characters
  - Wrote custom editor tooling to automatically generate blendspace assets from a shared parent/template asset, to avoid inconsistent settings between characters
- Created robust editor tooling for sharing/copying montage data (such as anim sequence durations, SFX/VFX/gameplay notifies, and play rates) from a single "parent" montage to N "child" montages
  - Allowed us to avoid repetitive, error prone tasks for shared hero and NPC abilities like "defeat"
- Wrote a system for syncing animation graph state machine and montage playback position from a single "leader" actor to N "follower" actors
  - Used for modular characters with synchronized weapon attachments, groups of characters that needed frame-perfect synchronized animations, and character/mount animation synchronization

### **Input/Controls**
- Worked with combat design lead to implement a fighting game-style input buffer and queuing system, so that the controls felt more responsive to players, and inputs during other abilities didn't feel like they were "dropped"
- Created a system of animation notify states for designers to author early breakout windows to end abilities early when transitioning directly into locomotion or other combo abilities
- Generalized input handling pattern across all of our supported platforms (console/gamepad, mobile/touch, and PC/mouse + keyboard) using Unreal's input system
- Implemented mouse + keyboard controls for aiming abilities based on cursor direction
- Wrote automatic input mode detection code based on last-detected input device (touch, gamepad, or mouse + keyboard), and logic to update the UI with platform-specific button icons depending on whether the player was using a mouse, Xbox, or PlayStation controller
- Wrote logic for input remapping across multiple platforms (mouse + keyboard, gamepad), as well as the saving/restoring logic and player-facing input remapping UI in the Settings menu

### **AI Behavior**
- Implemented factions/stance system using Unreal's [`FGenericTeamId`](https://docs.unrealengine.com/en-US/API/Runtime/AIModule/FGenericTeamId/) that allowed designers to assign attitudes between all the different NPC and player factions in our game
- Created a single AI controller for all NPCs in the game, with shared attack selection logic that performed ability activation based on designer-authored conditional checks (such as angle/distance to their combat focus, amount of health, etc.), weighted randomness, and prioritization
- Wrote custom C++ behavior tree classes (decorators, services, and tasks) to keep complex logic out of the Blueprint behavior tree graphs
- Created a component to handle combat pacing logic to coordinate attacks on the player between enemies of different types (e.g., ranged vs. melee), and prevent the player from feeling overwhelmed by enemies

### **Audio**
- Integrated [Audiokinetic's Wwise](https://www.audiokinetic.com/en/products/wwise/) into our gameplay systems, including use in our gameplay cues and hit-sparks system
- Configured the audio listener positioning/rotation for a 3rd person isometric action game
- Created system to switch footstep sounds based on floor physical materials using existing IK foot probes, as well as cheats/tools for audio designers to diagnose material problems with in-game surfaces

### **UI framework**
- Experienced in writing Unreal C++ for both [UMG](https://docs.unrealengine.com/en-US/umg-ui-designer-for-unreal-engine/) and [Slate](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/Slate/) UI controls
- Wrote gamepad/keyboard input handling system to limit input to the correct UI layer based on visibility
  - This allowed UI designers to place gamepad or keyboard triggered buttons on any screen, but only have them active if the buttons were visible to the player
- Created C++ base classes for HUD widgets that hooked into the Gameplay Ability System's costs and cooldowns for displaying character ability states in the UI
  - Optimized performance by storing ability charge data (resource, cooldown, and condition states) in a centralized cache, shaving 2ms off the total Slate tick time on our target hardware
- Created a re-usable scene capture rig for displaying 3D character and item models in the UMG UI (used in character selection, dialog, and shop UI)
- Mentored junior UI team members on how to use model/view UI architecture patterns, and virtualized/pooled widgets via Unreal's `UListView` and `UTileView` classes when displaying large numbers of items in the UI
- Created a set of re-usable controls for…
  - Tab-based menu navigation
  - Gamepad-triggerable buttons, with display of platform-specific button prompt images based on the player's hardware/input (e.g. "Esc" on PC, "B" on Xbox, and "O" on PlayStation)
  - Animated notification panels for displaying in-game loot, quest updates, and status notifications
  - Animated World Map for players to select missions and begin quests from
  - Modal dialogs for popup menus
  - Tutorial "coach marks" system that could be anchored to any UI elements, and offer interactive player instructions/walkthroughs
- Wrote custom [rich text decorators](https://docs.unrealengine.com/en-US/InteractiveExperiences/UMG/UserGuide/UMGRichTextBlock/) in C++ to bring our in-game text to life, including rich text tags for...
  - Status effect icons with tooltips to help player understanding of ability effects like burning/freezing
  - Translating Unreal Input actions names to the platform-specific button prompt images (e.g., to display the "A" button on Xbox controller or "LMB" on PC for the player's abilities)
  - Inline styling of text using HTML-style `<span>` tags that designers could use bold, italic, colors, hyperlinks, and differing font sizes directly in their ability descriptions
- Experienced in translating the UI artist's vision/style into Unreal UI materials
  - Created animated and procedural UI materials for the UI art team to use for visual effects like text blur, shadows, rounded image corners, and custom masking
  - Produced 2D UI VFX entirely in Unreal's material graph for calling player attention to "ultimate"-style abilities when they are fully charged
  - Authored and procedural UI animations, such as health bar "shaving"

### **Dialog**
- Worked with narrative team to create a custom dialog syntax and parser for designers to write scripts, based on Yarn
- Wrote tooling to generate Unreal localizable text from our scripts, so that narrative designers did not need to manage localization/string tables manually
- Created UI to display dialog trees, and offer player choices and see animated 3D character previews of speakers/NPCs

### **Technical art**
- Met with the TV show production team to understand the toon material/shader, and convert it to UE's material graph
- Created a destructibles/breakables system, with prediction and rollback if an ability activation that resulted in destroying the object was rejected

### **Tooling**
- Created custom editor blueprint widgets to display debugging information about abilities/actor states for all gameplay-relevant actors
  - This tooling worked both in-editor and in packaged builds, providing invaluable information to the QA/bug reporting teams
- Contributed to our cheats manager, allowing QA to bypass progression checks on the game server and platform, swap heroes or gear, and more easily test the game
- Wrote a Damage-Per-Second calculation tool for designers to record gameplay within Unreal, and view resulting damage output and sources in Excel
- Created a pipeline for designers to author all tuning values within Excel, export to JSON via VBA scripting and custom Ribbon buttons, and automated import into Unreal as data tables
- Partnered with our CTO and Director of Engineering to build a strongly-typed, plain-text data definition language that was able to produce validated JSON game data for all of the systems in our game, generate corresponding Unreal C++ code with Blueprint-able data asset types to import the JSON into the engine's asset registry, and express complex calculations and conditionals based on game state for use in abilities, dialog, AI, and other aspects of our game
  - This allowed our designers to keep much of our game's data in text rather than only relying on Blueprint, reducing the overall amount of runtime code to maintain in our game, as well as providing easier collaboration, reviewing, merging, and diff'ing (since our game data was in text rather than binary uassets)
  - I also wrote a translation layer between blueprint and our custom scripting, so that designers could always fall back on Blueprint if they needed it

### **Systems design (platform requests + UI)**
- Items/Inventory/Loadouts
  - Created the system for updating a character's loadout on-the-fly, allowing items to be picked up/equipped/unequipped mid-mission
  - Wrote the data types and C++ logic for equippable cosmetics, including character and weapon specific skins, pets, and chromas
- Quests
  - Collaborated with the platform team to create a robust and flexible Quest system, which allowed for cross-mission progression and...
    - Unlock requirements, and integration with the dialog system for players to be granted quests from speaking with NPC "operators"
    - Completion requirements (in the form of multiple "quest objectives" that designers could author)
    - Rewards (both XP and loot)
    - UI data for displaying different descriptions and flavor text based on the state of the quest completion
- Objectives
  - Our objectives system was our in-mission "to-do" list for players
- Missions
  - Requirements
  - Modifiers
- Difficulty
- Scaling NPCs' attributes and modifying their visuals
- Character Stats
- Crafting and Upgrading
- Stores (buying and selling inventory items)
- Hero progression/ability unlocks
- Checkpoints/Retry
  - Experienced with Unreal's [Server Travel](https://docs.unrealengine.com/en-US/InteractiveExperiences/Networking/Travelling/) feature, which I leveraged to create our checkpointing/retry system
  - When players reached specific designer-authored re-group points in the level, both player/character data and game state data would be cached, and if players were defeated, the world would reset, and the players would be teleported back to the most recent checkpoint and have the player and game states rest
- Tutorial
  - Worked together with the lead level designer to create our in-game combat tutorial that progressively unlocked the player's abilities and taught them how to use each ability
  - In addition to the in-game tutorial, I also created the menu walkthrough, which guided the player through the menus, teaching them how the game's interface works

Fun Fact: Our game actually started out its first year of life as a local multiplayer game built using the Unity engine. When we switched engines to Unreal, I helped port the game logic from Unity/C# to Unreal/C++, while trying to retain as much of our previous code, game data, and assets as possible.