# Implement-the-AI-random-movement


##  Aim
To create an AI character in Unreal Engine that roams randomly within a NavMesh area and chases the player when they come within a certain range, using Behavior Trees, Blackboard, and AI Perception.

##  Procedure

1. **Setup Navigation**
   - Add a `NavMeshBoundsVolume` to your level and scale it to cover the roamable area.
   - Press **P** to confirm the green nav area is visible (indicating navigable space).

2. **Create AI Character**
   - Create a Blueprint character (e.g., `BP_AIEnemy`) with a skeletal mesh and AIController class.
   - Create an AI Controller Blueprint (e.g., `BP_AIController`) and assign it to the character.

3. **Enable AI Perception**
   - In `BP_AIController`, add an `AIPerception` component.
   - Configure a **Sight** sense (set detection range, lose sight range, peripheral vision angle).
   - Bind `OnPerceptionUpdated` to update a blackboard value (e.g., `CanSeePlayer` and `PlayerActor`).

4. **Set Up Blackboard**
   - Create a Blackboard with the following keys:
     - `TargetLocation` (Vector)
     - `PlayerActor` (Object)
     - `CanSeePlayer` (Bool)

5. **Create Behavior Tree (BT_AI)**
   - Structure it like this:

     ```
     Root
     └── Selector
         ├── Sequence (Chase Player)
         │   ├── Blackboard Check: CanSeePlayer == true
         │   └── Move To: PlayerActor
         └── Sequence (Random Roam)
             ├── Task: Find Random Location → TargetLocation
             └── Move To: TargetLocation
     ```

6. **Custom Task: Find Random Location**
   - Create a new `BTTask_BlueprintBase` to get a random reachable point using:
     ```cpp
     UNavigationSystemV1::GetRandomReachablePointInRadius()
     ```
   - Set the result to the `TargetLocation` blackboard key.

7. **Test the AI**
   - Add a player character to the level.
   - Place the AI enemy in the map and assign its controller and behavior tree.
   - Press **Play**: the AI should roam when the player is far and chase the player when within sight.
  

## Output
<img width="1200" height="871" alt="444011063-8cc1b432-1809-4fb2-b82c-842d16b2f651" src="https://github.com/user-attachments/assets/53605514-8f56-416d-b35a-dbbe2e37a5ab" />

<img width="1016" height="428" alt="441818628-aac9fada-353e-4369-a7a1-8037059117b9" src="https://github.com/user-attachments/assets/4e7e0321-e999-4c47-92b5-5194e7a44920" />


<img width="1297" height="498" alt="441818665-0017652a-93f4-4168-b375-6389bb48b189" src="https://github.com/user-attachments/assets/58ae349c-5675-4a9e-8e4f-315671584441" />


## Result
The AI character roams randomly within a defined area. When the player enters its sight range, the AI stops roaming and begins to chase the player until the player is out of sight, after which it resumes roaming.
