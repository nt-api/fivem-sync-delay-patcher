# FiveM Sync Delay Patcher

This tool references citizenfx/fivem#1521 for context.

This utility allows FiveM server owners to modify the sync delay values in the `citizen-server-state-fivesv.dll` file, giving you control over how quickly entities (especially players) are synchronized across the network.

## Purpose

By adjusting sync delay values, you can:
- Improve responsiveness for combat-focused servers
- Increase headshot registration accuracy
- Reduce the delay between player actions and their effects
- Customize synchronization behavior based on your server's specific needs

## Usage

1. Run the application
2. Enter the path to your `citizen-server-state-fivesv.dll` file
3. Specify your desired sync delay value (default is 50ms)
4. Set a sync delay divisor (must be a power of 2, default is 4)
5. Optionally modify the lowest sync delay distance threshold (default 1225.0f)

## Benefits

This tool is particularly useful for:
- Headshot/one-shot servers where precise hit registration is critical
- Combat-focused roleplay servers
- Competitive PvP environments
- Any server where reducing network latency improves gameplay

## Proof of concept
With this tool, you can also counteract certain movement glitches - such as the speedboosting shown below - or other sync related issues

High sync delay (50ms):

https://github.com/user-attachments/assets/891a38c0-3971-4922-af4b-8579ee26ce83

Really low sync delay (0ms) - not recommended for larger servers

https://github.com/user-attachments/assets/5128622d-a25f-4cbf-ad89-918b9de65d9a



## Technical Details

This patcher modifies the compiled code implementing the sync delay logic from FiveM's server code:

See (https://github.com/citizenfx/fivem/blob/5042a3d4a57ac0924828b9a6fa13d62ab2ad3f34/code/components/citizen-server-impl/src/state/ServerGameState.cpp#L1267)
```cpp
// Source: https://github.com/citizenfx/fivem/blob/5042a3d4a57ac0924828b9a6fa13d62ab2ad3f34/code/components/citizen-server-impl/src/state/ServerGameState.cpp#L1267
auto syncDelay = 50ms; // <-- This is the default value we're patching

// Various conditions that modify the sync delay based on distance and entity type
// ...

if (dist < 35.0f * 35.0f) // <-- This distance threshold (1225.0f) can also be modified
{
    syncDelay /= 4; // <-- This divisor can be customized
}
```

## The tool allows you to customize:
- The base sync delay (default: 50ms)
- The divisor for nearby entities (default: 4)
- The distance threshold for applying the reduced sync delay (default: 35.0f * 35.0f = 1225.0f)

## Notes
- Lower sync delay values may increase network traffic
- Always backup your original DLL file before patching
- Values should be balanced according to your server's performance capabilities
- This tool modifies compiled code, use at your own risk
- Following the rejection of citizenfx/fivem#1521, this tool was created to implement the functionality by directly patching the compiled binary.
