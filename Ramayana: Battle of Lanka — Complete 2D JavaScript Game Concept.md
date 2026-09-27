# Ramayana: Battle of Lanka — Complete 2D JavaScript Game Concept

 If you want to turn the **Ramayana into a 2D JavaScript action/shooting game**, the best way to think about it is not simply "Rama shoots enemies." Instead, you can design it as a **story-driven 2D action game inspired by the Ramayana**, where gameplay, characters, levels, and progression are connected to the narrative.

 The following is a detailed game-design blueprint.

---

 # 1\. Game Overview

 ### Title

 **Ramayana: Battle of Lanka**

 ### Genre

 **2D action / ranged-combat / adventure game**

 You can call it a "2D shooting game" technically, because the player uses a ranged projectile system, but visually and thematically it would be closer to a **2D bow-and-arrow action game**.

 ### Platform

 For a JavaScript implementation:

 - Web browser
- Desktop
- Potentially mobile browsers later

 ### Technology

 A beginner-friendly stack could be:

```
HTML
  ↓
Canvas
  ↓
JavaScript
  ↓
Game Engine / Game Loop
  ↓
Characters + Enemies + Levels
```

 You could initially build it with **plain JavaScript + HTML Canvas**, without a game engine.

---

 # 2\. Core Game Idea

 The player controls **Rama** and progresses through a series of levels inspired by events and locations associated with the Ramayana.

 The core gameplay loop is:

```
Explore
   ↓
Encounter enemies
   ↓
Move / Aim
   ↓
Shoot arrows
   ↓
Hit enemies
   ↓
Defeat enemies
   ↓
Collect rewards
   ↓
Progress through level
   ↓
Story event
   ↓
Next level
```

 So the game combines:

 **Story + Exploration + Combat \+ Progression**

 rather than being only a shooting game.

---

 # 3\. Player Character — Rama

 The central playable character is **Rama**.

 In the game, Rama would have several properties.

```
const player = {
    name: "Rama",

    x: 100,
    y: 400,

    width: 50,
    height: 70,

    speed: 5,

    health: 100,
    maxHealth: 100,

    arrows: 20,

    score: 0
};
```

 These properties represent the game's internal state.

 For example:

```
player.x
```

 controls horizontal position.

```
player.y
```

 controls vertical position.

```
player.health
```

 controls the player's health.

---

 # 4\. Player Movement

 The player needs to control Rama.

 For example:

```
        ↑
        |
    ←   Rama   →
        |
        ↓
```

 Depending on the game style, you could use:

 ### Option A — Side-scrolling

 Rama moves primarily:

```
←──────────────→
```

 This is easier for a beginner.

 ### Option B — Top-down

 Rama can move:

```
       ↑
       |
←──── Rama ────→
       |
       ↓
```

 This gives you more freedom.

 ### Recommended for your first version

 Use **side-scrolling**.

 It's significantly easier to implement.

---

 # 5\. Bow and Arrow System

 Instead of a modern gun, the main ranged weapon is a **bow and arrow**.

 When the player presses a button:

```
Rama 🏹
   \
    \
     ─────────►
               Arrow
```

 JavaScript creates an arrow object.

```
function shootArrow() {
    arrows.push({
        x: player.x + player.width,
        y: player.y + 20,
        width: 30,
        height: 5,
        speed: 10
    });
}
```

 Now the arrow exists independently from Rama.

---

 # 6\. Arrow Movement

 Every frame, JavaScript updates the arrow:

```
function updateArrows() {
    for (let arrow of arrows) {
        arrow.x += arrow.speed;
    }
}
```

 Conceptually:

```
Frame 1:

Rama 🏹 ───►

Frame 2:

Rama 🏹 ───────►

Frame 3:

Rama 🏹 ───────────►
```

 The browser is repeatedly updating the game.

---

 # 7\. Enemy System

 The enemies represent opposing forces encountered during the game's fictionalized combat sequences.

 An enemy could have:

```
const enemy = {
    x: 700,
    y: 300,

    width: 50,
    height: 60,

    health: 30,
    maxHealth: 30,

    speed: 2
};
```

 Each enemy has:

 - Position
- Size
- Health
- Movement speed
- Attack behavior

---

 # 8\. Enemy Movement

 A simple enemy might move toward Rama.

```
function updateEnemy(enemy) {
    enemy.x -= enemy.speed;
}
```

 So:

```
Enemy
  👹
   |
   ↓

             Rama
              🏹
```

 The enemy gradually approaches the player.

 More sophisticated enemies could:

 - Move left/right
- Jump
- Dodge arrows
- Attack from a distance
- Protect another enemy
- Retreat
- Follow different movement patterns

---

 # 9\. Enemy Waves

 Instead of spawning one enemy at a time, you can create **waves**.

 For example:

 ### Wave 1

```
👹
```

 ### Wave 2

```
👹       👹
```

 ### Wave 3

```
👹   👹       👹
```

 ### Wave 4

```
👹 👹    👹    👹 👹
```

 JavaScript can manage this using an array:

```
let enemies = [];
```

 When a wave begins:

```
function spawnEnemy() {
    enemies.push({
        x: canvas.width,
        y: Math.random() * 400,
        health: 30,
        speed: 2
    });
}
```

---

 # 10\. Collision Detection

 This is one of the most important concepts in a 2D game.

 The computer needs to determine:

 > "Did this arrow hit this enemy?"

 Visually:

```
       Arrow
         ↓
─────────► 👹
           ↑
         Enemy
```

 You can use rectangle collision detection.

```
function collision(a, b) {
    return (
        a.x < b.x + b.width &&
        a.x + a.width > b.x &&
        a.y < b.y + b.height &&
        a.y + a.height > b.y
    );
}
```

 Then:

```
if (collision(arrow, enemy)) {
    enemy.health -= 10;
}
```

 If:

```
enemy.health <= 0
```

 the enemy is defeated.

---

 # 11\. Health System

 Rama needs a health system.

 For example:

```
Health

❤️ ❤️ ❤️ ❤️ ❤️
```

 Internally:

```
player.health = 100;
```

 If an enemy damages Rama:

```
player.health -= 10;
```

 The UI could display:

```
Health: 80
```

 or:

```
❤️❤️❤️❤️
```

 If health reaches zero:

```
if (player.health <= 0) {
    gameOver();
}
```

---

 # 12\. Score System

 You can reward the player for completing combat objectives.

 For example:

```
score += 10;
```

 The screen might show:

```
SCORE: 250
```

 You could give different scores for different achievements:

```
Enemy defeated       +10
Special enemy        +25
Wave completed       +50
Boss defeated        +500
```

 However, if the game is intended to represent a religious epic respectfully, consider using **progress, objectives, or achievements** alongside or instead of a purely arcade-style "kill score."

---

 # 13\. Levels

 This is where the Ramayana connection becomes much deeper.

 Instead of randomly generated stages, the game can have a **story progression**.

 For example:

```
LEVEL 1
Ayodhya
   ↓
LEVEL 2
Forest
   ↓
LEVEL 3
Kishkindha
   ↓
LEVEL 4
Journey toward Lanka
   ↓
LEVEL 5
Lanka
   ↓
LEVEL 6
Battle of Lanka
```

 These aren't meant to reproduce every event in the epic. They're a game structure inspired by its narrative.

---

 # 14\. Level 1 — Ayodhya

 ### Environment

 The opening level could introduce the player to the world.

 Visual elements:

```
        🏰
   ┌───────────┐
   │ Ayodhya   │
   └───────────┘

🌳     🏠     🌳
```

 ### Gameplay

 This level could be primarily tutorial-oriented.

 Teach:

 - Movement
- Jumping
- Aiming
- Shooting
- Interaction
- Basic combat

 Instead of immediately starting with combat, the player learns the controls.

 ### Objective

```
Objective:
Learn the controls and proceed to the next area.
```

---

 # 15\. Level 2 — Forest

 The forest provides a completely different visual environment.

```
🌳        🌳
   🌿 🌿
       🏹
      Rama

🌳              🌳
```

 ### Gameplay possibilities

 The player encounters environmental obstacles and hostile creatures/enemy units.

 You could introduce:

 - Platforming
- Jumping
- Basic enemies
- Environmental hazards

 The difficulty increases gradually.

---

 # 16\. Level 3 — Kishkindha

 This level introduces the Vanara allies.

 This is an important opportunity to change the gameplay.

 Instead of:

```
Rama vs enemies
```

 you now have:

```
Rama
  +
Allies
  ↓
Combat group
```

 For example:

```
                 👹
        👹                👹

       🐒       🏹       🐒
             Rama
              🧍
```

 Allies could provide different gameplay abilities.

 For example:

```
Hanuman
   ↓
Break obstacles

Vanara ally
   ↓
Distract enemies

Rama
   ↓
Long-range attack
```

---

 # 17\. Hanuman as an Ally

 Rather than making Hanuman simply another projectile-firing character, you can give him an **ally ability**.

 For example:

```
function useHanumanAbility() {
    // Clear a group of obstacles/enemies
}
```

 The player presses:

```
H
```

 and activates the ability.

 The screen could show an animation:

```
          🐒
        HANUMAN
           ↓
     ─────────────
       impact
```

 The exact mechanics can be fictionalized for gameplay rather than claiming that a game mechanic literally represents a religious tradition.

---

 # 18\. Level 4 — Journey Toward Lanka

 This could become an exploration-oriented stage.

 Instead of constant combat:

```
Explore
  ↓
Solve obstacle
  ↓
Continue
  ↓
Combat encounter
  ↓
Continue
```

 This gives the game variety.

 You don't want every level to be:

```
Enemy → Shoot → Enemy → Shoot → Enemy
```

 because that becomes repetitive.

---

 # 19\. Level 5 — Lanka

 Now the visual style changes dramatically.

```
             🏰
        ┌──────────┐
        │  LANKA   │
        └──────────┘

    👹      👹      👹

          🏹
         Rama
```

 You could introduce:

 - Stronger enemies
- Larger enemy waves
- New environments
- Traps
- Defensive structures
- More difficult combat

---

 # 20. Level 6 — Battle of Lanka

 This becomes the major combat section.

 The game could contain several stages:

```
Battle begins
     ↓
Enemy wave 1
     ↓
Enemy wave 2
     ↓
Elite enemy
     ↓
Enemy wave 3
     ↓
Major encounter
     ↓
Final confrontation
```

 This creates a sense of progression.

---

 # 21\. Boss Battles

 Bosses are stronger enemies with unique mechanics.

 A generic boss system could look like:

```
const boss = {
    name: "Boss",
    health: 1000,
    maxHealth: 1000,

    phase: 1,

    x: 700,
    y: 200
};
```

 The boss health bar could appear at the top:

```
BOSS
████████████████████
```

 As health decreases:

```
████████████████
```

 then:

```
██████████
```

 then:

```
████
```

---

 # 22\. Ravana as the Final Encounter

 If Ravana is included, this should be treated differently from ordinary enemies.

 Instead of making him simply:

```
Enemy HP = 5000
```

 you could make the encounter a **story-driven final confrontation** with multiple phases.

 For example:

```
Phase 1
Combat
   ↓
Phase 2
Avoid attacks
   ↓
Phase 3
Use strategy
   ↓
Phase 4
Final confrontation
   ↓
Story conclusion
```

 This gives the encounter narrative significance rather than treating Ravana as merely a high-health target.

---

 # 23\. Multiple Boss Phases

 A boss can change behavior as health decreases.

```
if (boss.health < 700) {
    boss.phase = 2;
}

if (boss.health < 400) {
    boss.phase = 3;
}
```

 Then:

```
if (boss.phase === 1) {
    basicAttack();
}

if (boss.phase === 2) {
    strongerAttack();
}

if (boss.phase === 3) {
    specialAttack();
}
```

 This is a very useful JavaScript/game-programming concept.

---

 # 24\. Power-Ups

 Power-ups give temporary abilities.

 For example:

```
🏹  Arrow Power
❤️  Health
⚡  Speed
🛡️  Protection
```

 The player collects them by touching them.

```
if (collision(player, powerUp)) {
    player.health += 20;
}
```

 You could have:

 ### Health Power-Up

```
❤️ +25 health
```

 ### Speed Power-Up

```
⚡ movement speed × 2
```

 ### Arrow Power-Up

```
🏹 faster arrows
```

 ### Shield

```
🛡️ temporary damage reduction
```

---

 # 25\. Animated Sprites

 A static image isn't enough for a polished game.

 Instead of:

```
Rama.png
```

 you could have a sprite sheet:

```
┌────┬────┬────┬────┐
│ 🧍 │ 🧍 │ 🧍 │ 🧍 │
└────┴────┴────┴────┘
```

 Each frame represents a different animation.

 For example:

```
Idle
 ↓
Walk 1
 ↓
Walk 2
 ↓
Walk 3
 ↓
Walk 4
```

 JavaScript displays a different frame every few milliseconds.

---

 # 26\. Shooting Animation

 When Rama shoots:

```
Frame 1

Rama 🧍
    🏹

Frame 2

Rama 🏹
     \

Frame 3

Rama 🏹 ─────►
```

 This makes the game feel much more responsive.

---

 # 27\. Enemy Animations

 Enemies can have:

```
Idle
Walk
Attack
Hit
Defeat
```

 For example:

```
enemy.state = "walking";
```

 When hit:

```
enemy.state = "hit";
```

 When defeated:

```
enemy.state = "defeated";
```

 The rendering system then chooses the appropriate animation.

---

 # 28\. Sound Effects

 Sound can make a huge difference.

 Possible sounds:

```
🏹 Bow release
💥 Impact
👣 Footsteps
⚔️ Combat
❤️ Health pickup
🎵 Background music
🏆 Level completion
```

 JavaScript can play audio:

```
const arrowSound = new Audio("arrow.mp3");

function shootArrow() {
    arrowSound.currentTime = 0;
    arrowSound.play();
}
```

 For a culturally sensitive game, the music should also be chosen carefully rather than using religious material merely as generic combat sound effects.

---

 # 29\. Background Music

 Each level can have a different atmosphere.

 For example:

```
Ayodhya
   ↓
Peaceful / atmospheric

Forest
   ↓
Nature / exploration

Kishkindha
   ↓
Adventure

Lanka
   ↓
Tense / dramatic

Final encounter
   ↓
Epic / dramatic
```

 The goal is to create an atmosphere appropriate to the scene.

---

 # 30\. Game UI

 The player needs information during gameplay.

 A basic interface might look like:

```
┌─────────────────────────────────────────┐
│ ❤️❤️❤️       SCORE: 250       LEVEL 3 │
│                                         │
│                                         │
│              👹     👹                  │
│                                         │
│                     🏹                  │
│                    RAMA                 │
│                                         │
└─────────────────────────────────────────┘
```

 The UI can show:

 - Health
- Score
- Current level
- Objectives
- Ability cooldown
- Boss health
- Pause button

---

 # 31\. Objectives

 Don't make the player simply kill everything.

 Instead, give objectives:

```
OBJECTIVE

✓ Reach the forest
✓ Defeat the enemy wave
□ Protect the allied unit
□ Reach the next area
```

 This makes the game feel more like an adventure.

---

 # 32\. Story System

 You can add small story scenes between levels.

 For example:

```
┌──────────────────────────────┐
│                              │
│          STORY SCENE         │
│                              │
│        [Character art]       │
│                              │
│     Story narration...       │
│                              │
│            [NEXT]            │
└──────────────────────────────┘
```

 JavaScript can control the dialogue:

```
const dialogue = [
    "Story text...",
    "Another story line...",
    "The journey continues..."
];
```

 The player presses a button to continue.

---

 # 33\. Game States

 One of the most important programming concepts is the **game state**.

 Your game might have:

```
let gameState = "menu";
```

 Possible states:

```
menu
 ↓
playing
 ↓
paused
 ↓
levelComplete
 ↓
story
 ↓
gameOver
```

 For example:

```
if (gameState === "playing") {
    updateGame();
    drawGame();
}
```

 If paused:

```
if (gameState === "paused") {
    drawPauseScreen();
}
```

 This prevents the entire game from continuing while the pause menu is open.

---

 # 34\. The Game Loop

 This is the heart of your JavaScript game.

```
function gameLoop() {

    update();

    draw();

    requestAnimationFrame(gameLoop);
}

gameLoop();
```

 Conceptually:

```
             ┌─────────────┐
             │    INPUT    │
             └──────┬──────┘
                    ↓
             ┌─────────────┐
             │    UPDATE   │
             └──────┬──────┘
                    ↓
             ┌─────────────┐
             │   COLLISION │
             └──────┬──────┘
                    ↓
             ┌─────────────┐
             │    DRAW     │
             └──────┬──────┘
                    ↓
             ┌─────────────┐
             │ NEXT FRAME  │
             └──────┬──────┘
                    │
                    └──────────►
```

 This happens many times per second.

---

 # 35\. How the JavaScript Architecture Could Look

 For a larger project, don't put everything into one giant JavaScript file.

 A better structure is:

```
ramayana-game/
│
├── index.html
│
├── css/
│   └── style.css
│
├── js/
│   ├── main.js
│   ├── player.js
│   ├── enemy.js
│   ├── arrow.js
│   ├── collision.js
│   ├── level.js
│   ├── boss.js
│   ├── input.js
│   ├── audio.js
│   └── game.js
│
├── assets/
│   ├── images/
│   ├── sprites/
│   ├── backgrounds/
│   └── audio/
│
└── levels/
    ├── ayodhya.js
    ├── forest.js
    ├── kishkindha.js
    └── lanka.js
```

 This is much easier to maintain than putting 5,000 lines into `game.js`.

---

 # 36\. Object-Oriented Approach

 JavaScript classes can represent game entities.

 For example:

```
class Player {

    constructor(x, y) {
        this.x = x;
        this.y = y;
        this.health = 100;
        this.speed = 5;
    }

    move() {
        // movement logic
    }

    shoot() {
        // shooting logic
    }

    draw() {
        // rendering logic
    }
}
```

 Then:

```
const rama = new Player(100, 400);
```

 You can similarly create:

```
class Enemy {}
class Arrow {}
class Boss {}
class PowerUp {}
```

 This teaches an important JavaScript concept: **classes and objects**.

---

 # 37\. The Complete Gameplay Cycle

 Putting everything together:

```
                   START
                     ↓
                  MAIN MENU
                     ↓
                  STORY
                     ↓
                  LEVEL 1
                 AYODHYA
                     ↓
                  LEVEL 2
                  FOREST
                     ↓
                  LEVEL 3
                KISHKINDHA
                     ↓
                  LEVEL 4
             JOURNEY TO LANKA
                     ↓
                  LEVEL 5
                   LANKA
                     ↓
                  LEVEL 6
               BATTLE OF LANKA
                     ↓
              FINAL ENCOUNTER
                     ↓
               STORY ENDING
                     ↓
                GAME COMPLETE
```

---

 # 38\. What Happens Inside One Level?

 Suppose you're playing the Lanka level.

 The actual program might do this:

```
Player enters level
        ↓
Load background
        ↓
Load Rama sprite
        ↓
Load enemies
        ↓
Start game loop
        ↓
Read keyboard/mouse input
        ↓
Move Rama
        ↓
Spawn enemy
        ↓
Enemy moves
        ↓
Player shoots arrow
        ↓
Arrow moves
        ↓
Collision detection
        ↓
Enemy takes damage
        ↓
Enemy defeated
        ↓
Score/progress updated
        ↓
More enemies spawn
        ↓
Wave completed
        ↓
Story event
        ↓
Next section
```

 That's the fundamental architecture of the game.

---

 # 39\. A Better Way to Think About "Shooting"

 Technically, your game has a **projectile system**.

 That's a more useful programming concept than thinking specifically about guns.

```
Player
  ↓
Creates projectile
  ↓
Projectile moves
  ↓
Projectile checks collision
  ↓
Target receives damage/effect
```

 For this game:

```
Rama
 ↓
Arrow
 ↓
Enemy
 ↓
Damage
```

 This same programming system could later support:

```
Arrow
Fireball
Magic projectile
Enemy projectile
Special ability
```

 So you're learning a reusable game-development system.

---

 # 40\. Respectful Design

 This is particularly important for a Ramayana-inspired game.

 The Ramayana is not merely a fictional action franchise; it is a **religiously and culturally significant epic with many traditions and interpretations**.

 Therefore, one design approach is to distinguish between:

 ### Story

 The game can draw inspiration from:

 - Characters
- Locations
- Major narrative events
- Themes
- Relationships
- Journey structure

 ### Gameplay

 Gameplay can use fictionalized mechanics such as:

 - Enemy waves
- Health bars
- Power-ups
- Boss phases
- Objectives
- Checkpoints

 The gameplay mechanics don't have to be presented as literal representations of the religious tradition.

 For example, instead of saying:

 > "This power-up represents a particular sacred concept."

 you can simply make it a **gameplay ability inspired by the character or story context**.

---

 # 41\. An Example of a Complete Level

 Let's imagine **Level 5: Lanka**.

 ### Beginning

```
STORY

Rama and his allies have reached Lanka.

        [Continue]
```

 ### Gameplay starts

```
LEVEL 5 — LANKA

Objective:
Reach the next area.
```

 ### Enemy wave

```
👹       👹
       👹

             🏹
            Rama
```

 Player shoots:

```
Rama 🏹 ───────► 👹
```

 Collision:

```
Enemy HP
████████
   ↓
██████
   ↓
██
   ↓
Defeated
```

 ### Next wave

 More difficult enemies appear.

 ### Ally event

 Hanuman assists during an obstacle sequence.

 ### Final section

 The player reaches a major encounter.

 ### Boss encounter

 A large health bar appears:

```
FINAL ENCOUNTER
████████████████████
```

 The player must learn the boss's attack patterns and complete the encounter.

 ### Ending

 Instead of immediately displaying:

```
YOU WIN!
+5000 SCORE
```

 you can transition into a story scene:

```
LEVEL COMPLETE

The battle has ended.

             [Continue]
```

 This helps preserve the distinction between **game mechanics and the source narrative**.

---

 # 42\. What You Would Learn by Building This

 This project is particularly useful for learning JavaScript because it combines many concepts.

 ### JavaScript fundamentals

 You would practice:

```
Variables
↓
Functions
↓
Arrays
↓
Objects
↓
Classes
↓
Events
↓
Loops
↓
Conditionals
```

 ### Game development

 Then:

```
Game loop
↓
Input
↓
Rendering
↓
Animation
↓
Collision detection
↓
Physics
↓
Game states
↓
Level management
```

 ### More advanced concepts

 Eventually:

```
Sprite sheets
↓
Particle effects
↓
Audio management
↓
AI
↓
Camera systems
↓
Save/load
↓
Performance optimization
```

---

 # 43\. Recommended Development Order

 **Don't try to build the entire Ramayana game at once.**

 Build it in stages.

 ### Stage 1 — Canvas

 Create:

```
HTML
+
Canvas
+
JavaScript
```

 and draw a rectangle representing Rama.

 ### Stage 2 — Movement

 Make Rama move:

```
← → ↑ ↓
```

 ### Stage 3 — Arrow

 Add:

```
🏹 →
```

 ### Stage 4 — Enemy

 Create one enemy.

```
Rama 🏹 ─────► 👹
```

 ### Stage 5 — Collision

 Make arrows hit enemies.

 ### Stage 6 — Health

 Add:

```
Rama ❤️❤️❤️
Enemy █████
```

 ### Stage 7 — Multiple enemies

 Add enemy waves.

 ### Stage 8 — Art

 Replace rectangles with sprites.

 ### Stage 9 — Animation

 Add:

 - Walking
- Shooting
- Enemy attacks
- Hit effects

 ### Stage 10 — Levels

 Create:

```
Ayodhya
Forest
Kishkindha
Lanka
```

 ### Stage 11 — Story

 Add dialogue and transitions.

 ### Stage 12 — Boss

 Create the final encounter.

 ### Stage 13 — Audio

 Add sound and music.

 ### Stage 14 — Polish

 Add:

 - Menus
- Pause
- Settings
- Better animations
- Effects
- Responsive controls
- Mobile support

---

 # 44\. The Final Mental Model

 The entire project can ultimately be understood as five major systems:

```
                  RAMAYANA GAME
                        │
       ┌────────────────┼────────────────┐
       ↓                ↓                ↓
     STORY           GAMEPLAY          VISUALS
       │                │                │
 Characters          Movement          Sprites
 Locations           Shooting          Animation
 Events              Enemies           Backgrounds
 Dialogue             Collision         Effects
       │                │                │
       └────────────────┼────────────────┘
                        ↓
                     ENGINE
                        │
              Game Loop + Input
                        │
                        ↓
                    JAVASCRIPT
```

 And the most important programming loop is:

```
INPUT
  ↓
UPDATE
  ↓
COLLISION
  ↓
RENDER
  ↓
INPUT
  ↓
UPDATE
  ↓
...
```

 So **"Ramayana: Battle of Lanka" isn't just a story placed inside a shooting game**. A well-designed version would use the Ramayana-inspired world to give meaning and structure to the game's **levels, characters, objectives, environments, abilities, and progression**, while JavaScript provides the machinery that makes all of those things interactive.
