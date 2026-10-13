<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:1C1917,50:7C2D12,100:F59E0B&height=260&section=header&text=Tank%20Battle%203D&fontSize=52&fontColor=FCD34D&fontAlignY=38&animation=fadeIn&desc=A%203D%20tank%20defense%20game%20built%20from%20scratch%20in%20Python%20and%20PyOpenGL&descSize=17&descAlignY=58&descAlign=50" alt="Tank Battle 3D" width="100%"/>

<a href="https://github.com/sabikswanan/Tank-Battle-"><img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=21&duration=2600&pause=900&color=F59E0B&center=true&vCenter=true&width=760&lines=%3E+Drive.+Aim.+Fire.;%3E+Defend+your+base+through+endless+waves;%3E+Arcing+shells+%C2%B7+power-ups+%C2%B7+minimap+%C2%B7+2+cameras;%3E+~1%2C000+lines+of+pure+Python+%2B+OpenGL" alt="Drive. Aim. Fire. Defend your base through endless waves."/></a>

<br/>

<a href="https://github.com/sabikswanan/Tank-Battle-/blob/main/tank_battle%20video.mp4"><img src="https://img.shields.io/badge/%E2%96%B6%20WATCH-GAMEPLAY%20VIDEO-EA580C?style=for-the-badge&labelColor=0B0F14" alt="Watch the gameplay video"/></a>

<img src="https://img.shields.io/badge/FEATURES-14-F59E0B?style=for-the-badge&labelColor=0B0F14" alt="14 features"/>
<img src="https://img.shields.io/badge/WAVES-ENDLESS-EA580C?style=for-the-badge&labelColor=0B0F14" alt="Endless waves"/>
<img src="https://img.shields.io/badge/CAMERAS-2-65A30D?style=for-the-badge&labelColor=0B0F14" alt="2 camera modes"/>
<img src="https://img.shields.io/badge/POWER--UPS-2-0EA5E9?style=for-the-badge&labelColor=0B0F14" alt="2 power-ups"/>

<img src="https://img.shields.io/badge/Python-3-3776AB?style=for-the-badge&logo=python&logoColor=white&labelColor=0B0F14" alt="Python"/>
<img src="https://img.shields.io/badge/OpenGL-PyOpenGL-5586A4?style=for-the-badge&logo=opengl&logoColor=white&labelColor=0B0F14" alt="PyOpenGL"/>
<img src="https://img.shields.io/badge/GLUT-Window%20%26%20Input-4D7C0F?style=for-the-badge&labelColor=0B0F14" alt="GLUT"/>
<img src="https://img.shields.io/badge/CSE423-Computer%20Graphics-334155?style=for-the-badge&labelColor=0B0F14" alt="CSE423 Computer Graphics"/>

<br/><br/>

**[Overview](#overview)** &nbsp;|&nbsp;
**[Controls](#controls)** &nbsp;|&nbsp;
**[Gameplay](#gameplay)** &nbsp;|&nbsp;
**[Features](#features)** &nbsp;|&nbsp;
**[Under the Hood](#under-the-hood)** &nbsp;|&nbsp;
**[Getting Started](#getting-started)** &nbsp;|&nbsp;
**[Author](#author)**

</div>

<br/>

<a name="overview"></a>

## 🎮 Overview

**Tank Battle 3D** is a wave-based tank defense game. You command a tank in a circular arena,
enemy tanks roll in from the far side, and your job is to keep both **your tank** and **your base**
alive. Every cleared round brings a bigger wave.

> [!NOTE]
> The entire game — rendering, physics, enemy AI, HUD and menus — lives in **one Python file of
> about 1,000 lines**, built only with the OpenGL and GLUT functions from the course lab templates.
> No game engine, no textures, no external assets.

Built for **CSE423 — Computer Graphics** at **BRAC University**.

<div align="center">

<a href="https://github.com/sabikswanan/Tank-Battle-/blob/main/tank_battle%20video.mp4"><img src="https://img.shields.io/badge/%E2%96%B6%20Watch%20the%20gameplay%20video-EA580C?style=for-the-badge&labelColor=0B0F14" alt="Watch the gameplay video"/></a>

</div>

<br/>

<a name="controls"></a>

## 🕹️ Controls

<table>
<tr>
<td valign="top" width="50%">

**Driving & combat**

| Key | Action |
|:---:|---|
| <kbd>W</kbd> / <kbd>S</kbd> | Drive forward / backward |
| <kbd>A</kbd> / <kbd>D</kbd> | Turn left / right |
| <kbd>Space</kbd> or <kbd>Left Click</kbd> | Fire a shell |

</td>
<td valign="top" width="50%">

**Camera & game**

| Key | Action |
|:---:|---|
| <kbd>V</kbd> | Switch camera (Orbit ↔ Follow) |
| <kbd>←</kbd> / <kbd>→</kbd> | Rotate the orbit camera |
| <kbd>P</kbd> | Pause / resume |
| <kbd>R</kbd> | Restart |

</td>
</tr>
</table>

<br/>

<a name="gameplay"></a>

## ⚔️ Gameplay

```mermaid
stateDiagram-v2
    direction LR
    [*] --> Battle: Round 1 · 3 enemies
    Battle --> RoundClear: all enemies destroyed
    RoundClear --> Battle: next round · 2 + round enemies
    Battle --> Paused: P
    Paused --> Battle: P
    Battle --> GameOver: tank HP 0 or base HP 0
    GameOver --> Battle: R · restart

    classDef fight fill:#EA580C,stroke:#7C2D12,stroke-width:2px,color:#FFFFFF
    classDef calm fill:#1C1917,stroke:#F59E0B,stroke-width:1.5px,color:#FDE68A
    classDef lost fill:#7F1D1D,stroke:#DC2626,stroke-width:2px,color:#FFFFFF
    class Battle fight
    class RoundClear, Paused calm
    class GameOver lost
```

### Waves get bigger every round

```mermaid
%%{init: {"themeVariables": {"xyChart": {"plotColorPalette": "#EA580C", "titleColor": "#F59E0B"}}}}%%
xychart-beta
    title "Enemy tanks per round"
    x-axis ["R1", "R2", "R3", "R4", "R5", "R6", "R7", "R8"]
    y-axis "Enemies" 0 --> 12
    bar [3, 4, 5, 6, 7, 8, 9, 10]
```

### Combat numbers

<table>
<tr>
<td valign="top" width="50%">

**You vs. them**

| Event | Effect |
|---|---|
| Your shell hits an enemy | **−25 HP** (enemies have 30) |
| Enemy destroyed | **+10 score** |
| Round cleared with base at full HP | **+25 bonus** |

</td>
<td valign="top" width="50%">

**Them vs. you**

| Event | Effect |
|---|---|
| Enemy shell hits your tank | **−12 HP** (of 100) |
| Enemy shell hits your base | **−8 HP** (of 100) |
| Tank or base reaches 0 HP | **Game over** |

</td>
</tr>
</table>

### Power-ups

| Pickup | Effect | Duration |
|---|---|:---:|
| 🟡 **Rapid Fire** | Time between shots drops from **0.45 s to 0.18 s** | 8 s |
| 🔵 **Speed Boost** | Tank moves **1.6×** faster | 8 s |

Two power-ups spawn at the start of every game and more appear during battle (up to four at once).
Active boosts show as pulsing icons with a countdown at the top of the screen.

### How enemies think

```mermaid
flowchart LR
    A(["Find nearest target<br/>tank or base"]) --> B{"Farther than<br/>160 units?"}
    B -- yes --> C(["Drive toward it"])
    B -- no --> D(["Hold position"])
    C --> E{"Within 500 units<br/>and reloaded?"}
    D --> E
    E -- yes --> F(["Fire an arcing shell<br/>reload 1.5 – 2.8 s"])
    E -- no --> A
    F --> A

    classDef step fill:#1C1917,stroke:#F59E0B,stroke-width:1.5px,color:#FDE68A
    classDef check fill:#7C2D12,stroke:#EA580C,stroke-width:1.5px,color:#FFFFFF
    classDef fire fill:#EA580C,stroke:#7C2D12,stroke-width:2px,color:#FFFFFF
    class A,C,D step
    class B,E check
    class F fire
    linkStyle default stroke:#F59E0B,stroke-width:1.8px
```

<br/>

<a name="features"></a>

## ✨ Features

<table>
<tr>
<td valign="top" width="50%">

#### 🎯 Combat
- Shells follow a **gravity-based arc**
- Enemies **chase and shoot** at the nearest target
- **Obstacles** block movement and the view
- Tank **flashes and shrinks** when hit

</td>
<td valign="top" width="50%">

#### 🧭 Camera & HUD
- **Orbit** and **Follow** camera modes
- **Minimap** in the corner
- **Capsule HP bars** that turn green → yellow → red and pulse below 40%
- Score shown as a **spinning, bobbing coin**

</td>
</tr>
<tr>
<td valign="top" width="50%">

#### 🌄 World
- Circular arena with animated, shimmering ground rings
- Gradient **sky dome** with a glowing sun and distant hills
- Fort-style **base** with towers, a pulsing beacon and orbiting orbs
- Crates topped with **floating, spinning gems**

</td>
<td valign="top" width="50%">

#### 🔁 Game flow
- **Endless waves** that grow every round
- **Rapid Fire** and **Speed Boost** power-ups
- **Pause** without closing the game
- **Game over** screen with instant restart

</td>
</tr>
</table>

<details>
<summary><b>All 14 features mapped to the code</b></summary>
<br/>

| # | Feature | Where it lives |
|:---:|---|---|
| 1 | Player tank (boxes + cylinder barrel) | `draw_tank()` |
| 2 | Turret moves with the tank body | `update()` |
| 3 | Enemy tanks chase the nearest target and fire from range | `update_enemies()`, `enemy_fire()` |
| 4 | Rounds that get harder each time | `next_round_if_clear()`, `spawn_wave()` |
| 5 | Shells arc through the air with gravity | `update_shells()` |
| 6 | Obstacles block view and movement | `draw_obstacles()`, `blocked()` |
| 7 | Base with its own health bar | `draw_hp_capsule()` |
| 8 | Tank flashes and shrinks when hit | `draw_player_tank()` |
| 9 | Two camera views (orbit + follow) | `setup_camera()` |
| 10 | Power-ups (rapid fire / speed boost) | `update_powerups()`, `draw_powerups()` |
| 11 | Minimap | `draw_minimap()` |
| 12 | Animated coin score | `draw_score_coins()`, `draw_coin()` |
| 13 | Pause key | `keyboard_down()` |
| 14 | Game over screen and restart | `draw_hud()`, `reset_game()` |

</details>

<br/>

<a name="under-the-hood"></a>

## 🔧 Under the Hood

| Area | Approach |
|---|---|
| **Game loop** | All animation runs in GLUT's `idle()` callback using a `time.time()` delta, so enemies, shells and effects move at the same speed on any machine |
| **Projectile physics** | Each shell carries a 3D velocity; gravity (`GRAV = 500`) pulls its vertical speed down every frame to create the arc |
| **Collision** | Distance checks between shells, tanks, the base and obstacles, plus an arena boundary test |
| **Enemy AI** | Target selection by distance, steering toward the target, and a randomised reload timer |
| **Rendering** | Every object is built from OpenGL primitives — quads, cubes, cylinders, spheres and points — with no textures or lighting |
| **Constraints** | Only GL/GLUT functions from the lab templates; `glutTimerFunc` deliberately not used |

<br/>

<a name="getting-started"></a>

## 🚀 Getting Started

```bash
git clone https://github.com/sabikswanan/Tank-Battle-.git
cd Tank-Battle-
pip install PyOpenGL PyOpenGL_accelerate
python tank_battle.py
```

> [!TIP]
> Press <kbd>V</kbd> right away to try the follow camera, and grab a 🟡 power-up early — rapid fire
> makes the first waves much easier.

<br/>

## 🗺️ Roadmap

- [ ] Sound effects for firing, hits and explosions
- [ ] New enemy types with different speed, armour and range
- [ ] Persistent high-score table
- [ ] Hold-to-drive movement for smoother control

<br/>

<a name="author"></a>

## 👤 Author

<div align="center">

### Sabik Swanan

B.Sc. in Computer Science · BRAC University

<br/>

<a href="https://github.com/sabikswanan"><img src="https://img.shields.io/badge/GitHub-sabikswanan-7C2D12?style=for-the-badge&logo=github&logoColor=white&labelColor=0B0F14" alt="GitHub"/></a>
<a href="https://www.linkedin.com/in/sabikswanan"><img src="https://img.shields.io/badge/LinkedIn-sabikswanan-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=0B0F14" alt="LinkedIn"/></a>
<a href="https://mail.google.com/mail/?view=cm&fs=1&to=sabikswanan@gmail.com"><img src="https://img.shields.io/badge/Email-sabikswanan%40gmail.com-EA580C?style=for-the-badge&logo=gmail&logoColor=white&labelColor=0B0F14" alt="Email"/></a>

<br/><br/>

<sub>⭐ Star this repo if you enjoyed the game</sub>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:F59E0B,50:7C2D12,100:1C1917&height=130&section=footer" alt="" width="100%"/>

</div>
