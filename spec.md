# Specification Document: *Cloud Jump*

## 1. General Description
*Cloud Jump* is a simple 2D vertical platformer game built with HTML5 Canvas and JavaScript. The player controls an anime girl character in a retro, pixel-style environment. The objective is to climb as high as possible by jumping from cloud to cloud, while the world scrolls upward toward the sky. The score increases as the player ascends. The game ends when the player falls below the visible screen after the starting ground platform has disappeared.

---

## 2. Core Gameplay Mechanics
- **Player Character**: Represented by a placeholder pink square (with the possibility of sprite replacement later).  
- **Controls**:
  - `←` Move left.  
  - `→` Move right.  
  - `Space` Jump. Jump triggers on a fresh press and is allowed when standing on a platform or within coyote-time after stepping off. A short jump-buffer allows a slightly early press to still register when landing.  
- **Ground Platform**:
  - Exists only at the beginning.  
  - Scrolls off-screen as the player climbs and then disappears permanently.  
- **Cloud Platforms**:
  - Initially generated at intervals around a target spacing with small random variation.  
  - Respawn near the top with a small random vertical offset (spacing is not strictly enforced over time).  
  - Move horizontally across the screen with random directions and speeds (which scale with level).  
  - Player stands on and moves with the clouds when landed.  
- **Scoring**:  
  - Score increases based on vertical progress (the higher the climb, the more points).  
- **Difficulty Scaling**:
  - As the player’s score increases, cloud spacing grows slightly and cloud movement speeds increase.  
  - The game tracks levels (e.g., Level 1, Level 2…), based on score milestones.  
- **Level Up Feedback**:
  - A "LEVEL UP!" popup briefly flashes, floating upward and fading out, whenever the player levels up.  
- **Game Over**:
  - Occurs when the player falls below the bottom of the screen.  
  - Displays a semi-transparent overlay with:  
    - Final Score.  
    - Final Level.  
    - A clickable **Restart button**.  
    - An instruction text: *"Press SPACE to Restart"*.  
- **Restart**:
  - Triggered either by pressing **Space** or clicking the **Restart button**.  
  - Includes a **200ms cooldown** to prevent accidental jumps at the moment of restarting.  
  - Fresh press handling prevents an immediate jump from a Space key that remained held.  

---

## 2.1 Additional Quality-of-Life Mechanics
- **Coyote Time**: After walking off a platform, the player can still jump for a short window (≈120ms).  
- **Jump Buffer**: A jump press made slightly before landing (≈100ms) will trigger on landing.  
- **Time-Based Physics**: Movement and gravity scale with frame delta time to keep behavior consistent across frame rates.  
- **Pause & Mute**: Toggle pause with `P` and mute with `M`.  
- **Audio Feedback**: Lightweight SFX for jump, land, and level-up with a mute option.  
- **Mobile Touch Controls**: On-canvas buttons for Left/Right/Jump on touch devices.  
- **High Score**: Best score persisted via localStorage and shown in HUD and Game Over.  
- **Minor VFX**: Small particles/dust for jump and land events.  

---

## 3. User Scenarios

### 3.1 Starting the Game
- **Scenario**: The player loads the page in a browser.  
- **Result**: The game starts automatically, with the character standing on the ground platform.  

### 3.2 Climbing
- **Scenario**: The player jumps on clouds to ascend.  
 - **Result**: The background scrolls downward, simulating vertical movement into the sky. The score increases as progress is made. Cloud spacing varies slightly due to randomization.  

### 3.3 Riding Moving Clouds
- **Scenario**: The player lands on a cloud that moves left or right.  
- **Result**: The player moves along with the cloud’s velocity, preventing them from slipping off.  

### 3.4 Leveling Up
- **Scenario**: The player surpasses a score threshold (e.g., every 1000 points).  
 - **Result**: The Level increases, clouds move faster, and spacing grows. A "LEVEL UP!" popup appears briefly, with a small sound effect if not muted.  

### 3.5 Falling
- **Scenario**: The player misses a platform and falls below the screen.  
- **Result**: The game ends, displaying Game Over UI with final score, level, and restart options.  

### 3.6 Restarting
- **Scenario A**: The player clicks the Restart button.  
- **Scenario B**: The player presses Space.  
- **Result**: The game resets to the initial state with the ground platform visible, the character standing on it, score reset to zero, and difficulty reset. Space-jump input is disabled for 200ms to prevent accidental jumps, and a fresh-press is required before the first jump.  

### 3.7 Mobile Play
- **Scenario**: The player opens the game on a touch device.  
- **Result**: On-screen buttons allow moving left/right and jumping.  

### 3.8 Pausing and Muting
- **Scenario**: The player presses `P` or `M`.  
- **Result**: `P` toggles pause (freezes simulation). `M` toggles mute for sound effects.  

---

## 4. Functional Requirements
1. **Player Movement**
   - Must support left/right movement and jumping.  
   - Must obey gravity and collide correctly with ground and cloud platforms.  

2. **Platforms**
   - Ground must only exist at the beginning.  
  - Clouds must move horizontally and respawn once they scroll out of view.  
  - Initial clouds are placed around a target spacing with small random variation; respawned clouds appear near the top with a small random vertical offset.  
   - Player must “stick” to moving clouds when standing on them.  

3. **Scoring and Levels**
   - Score must increase based on vertical climbing.  
  - Levels must scale difficulty (faster and further apart clouds).  
   - Level-up popup must be shown upon level increment.  
  - High score (best) must be stored and displayed.  

4. **Game Over**
   - Trigger when player falls below the canvas.  
   - Must display final score, level, and restart options.  

5. **Restart**
   - Must reset score, level, and environment.  
   - Must prevent unintended jump at restart with a 200ms cooldown.  
   - Must allow restart via Space or Restart button.  
  - Must require a fresh press before the first jump after restarting.  

6. **Input & Control Enhancements**
  - Support coyote time and a jump buffer.  
  - Support time-based physics using delta time.  
  - Provide mobile touch controls.  
  - Provide pause and mute toggles.  
  - Provide basic SFX for jump/land/level-up with mute control.  

---

## 5. Non-Functional Requirements
- **Platform**: Runs in modern web browsers (tested in Chrome).  
- **Performance**: Must maintain smooth animation at ~60 FPS.  
- **Input Responsiveness**: Player input should be near-instantaneous.  
- **Accessibility**: Simple controls (arrow keys + space), visual indicators for feedback.  
 - **Robustness**: Behavior should remain consistent across varying frame rates due to time-based physics.  
