# Space Explorer
This is a very ambitious game request. Let me break down what's needed:

Launch phase - Rocket on Earth, steerable with arrow keys, thrust with up arrow
Space phase - When rocket exits screen top, zooms out to space view with planets (Earth, Moon, Monster Planet)
Planet landing phase - When approaching a planet, zoom in, land on it
Character phase - Press A to exit rocket, control character with arrow keys, space to jump, W to shoot water, S for shield
Monster system - Only on Monster Planet, monsters shoot back, water kills them in 2 hits, crown scoring
Lives system - 3 hearts top right
Level system - 100 levels, level 100 has boss
Leaderboard - Firebase, top 3 with medals
Firebase integration - Save game states

This is incredibly complex for a single HTML file. I need to architect this carefully as a Canvas-based game with distinct phases and state management, then build it directly rather than planning further.
I'm mapping out the core game states: launching from a planet surface with thrust and steering controls, transitioning to a space view with multiple planets, handling landing animations, exploring planet surfaces with enemies, and displaying end screens with leaderboard data. The progression spans three planets—Earth as the starting point, the Moon as a peaceful zone, and a Monster Planet with increasing difficulty that culminates in a boss fight at level 100. I'll integrate Firebase Realtime Database via CDN to persist game progress and track top scores. Writing the HTML structure... I'm realizing this is a substantial project to tackle all at once. Let me step back and think about what's actually feasible to build in one go that'll work properly. I should probably check with Thomas about the Firebase configuration since the leaderboard won't function without it, but I can build the game with localStorage as a fallback so it still works either way. Let me map out the full structure I need: constants and configuration at the top, Firebase setup with error handling, then the game logic and rendering.
I'm thinking through the essential features for a first version—rocket launching with thrust, the space view with planets, surface exploration, monsters to fight, and a working leaderboard. I'll use an 800x600 canvas that scales responsively, generate planets deterministically in space, and spawn monsters based on the current level. Let me start writing the complete game code now. Setting up the HTML structure with a canvas element, then initializing the canvas context and handling window resizing to keep the game responsive. Now I'm setting up the input system...
I'm tracking keyboard input with separate objects for current key states and keys that were just pressed this frame, then defining all the different game states the application can be in—menu, launch, space exploration, planet approach, surface walking, game over, leaderboard, and name input—before initializing the game to start at the menu.
Now I'm setting up the core game variables like level, lives, and crown collection, along with tracking which planet the player is currently on and whether they're in the rocket, then defining the planet data with their visual properties like colors and sizes. Setting up the game objects...
Now I'm defining the physics constants and initializing the planet surface with the rocket and character positioned on the ground, then setting up the camera system.
Setting up platforms and obstacles... Writing monster spawning logic...
Now I'm setting up the space phase with a spaceship object that tracks position, velocity, and angle, then defining a function to transition into space by positioning the ship near the departure planet. I'm also starting to add collision detection helpers.
Setting up game loop... Still writing game physics... Writing game physics... Still writing game logic... Writing game physics and collision logic... Writing game logic... Writing game physics... I'm checking if player projectiles hit any monsters, removing the projectile and incrementing the hit counter, then marking the monster as dead if it's taken enough damage and spawning a crown animation when defeated. After that, I'm filtering out monsters that have finished their death animation.
Now I'm updating the monster projectiles by moving them and decrementing their lifetime, then handling collision detection when the player gets hit by enemy fire, with special logic for when the shield is active. Writing game logic...
Now I'm setting up the render function to handle different game states, switching between menu, launch, space, approaching, surface, game over, and name input screens. For the menu specifically, I'm drawing a starfield background, then layering the title text in bold white, followed by instructions in a lighter blue color.
Setting up the stars array... Writing the launch scene... Finishing the rocket's visual details... Finishing the flame effect and moving into the space rendering section where I'm drawing stars and planets with glow effects. Writing the canvas rendering... Writing the approaching animation... Still writing the rendering code... Writing the rendering pipeline... Finishing the crown animation rendering with alpha blending, then drawing the HUD and control hints that change based on whether the player is in the rocket or not...
Writing character rendering... Still drawing the character... Writing the boss monster visuals... Writing monster graphics... Writing the HUD display...
Now I'm setting up the game over screen with variables for player name input and leaderboard submission, then rendering a semi-transparent overlay with stars in the background. The display shows either a victory message with a trophy if the level exceeds 100, or a standard game over message, both in large bold text centered on the screen.
Rendering the score display and name input form... Writing the leaderboard display... Writing game state logic... I'm loading the leaderboard from local storage as a fallback, then setting up the Firebase configuration placeholder for users to fill in their own credentials. Now I'm realizing the code structure needs some work—let me rewrite this as a complete, properly organized file that actually functions well.

Das neue Space Explorer ist fertig. Hier eine Übersicht aller Neuerungen:

🌋 Magmaplanet & Monsterburg

Neuer 4. Planet im Weltraum, mit orangefarbener Lava-Atmosphäre
Monsterburg rechts, mit Türmen, Zinnen, Burgfenstern und Schild
Lavagraben links — blockiert den linken Eingang
Zugbrücke: senkt sich automatisch, wenn Magmamonster sich annähern
Sprungtreppe rechts: 7 stufenförmige Plattformen über die Burg zum rechten Eingang
12 kleine Magmalöcher verteilt — mit Leertaste überspringen
Weniger Gravitation (0.28) → man springt höher

👿 Monsterboss (30 Leben)

Eigene Boss-Szene (Burginneres) mit Fackeln, Kachelboden, Dunkelatmosphäre
Boss-Sprite mit Krone, Schwert am Rücken, Kanone im Arm, glühende Augen
10 Kanonenschüsse gleichzeitig in Fächerform, werden schneller in Phase 2+3
Sieg: Explosion, Meldung, +30 Kronen, Rückflug zur Erde

🛡 Schild-System

Schild hält 100 Treffer, dann kaputt
Glücksbaum (links jedes Planeten, goldene Früchte) — 10 Sekunden danebenstehen = Schild voll aufgeladen
Schild-Balken oben rechts neben den Herzen

🎵 Musik & UI

Space-Theme beim Menü (Web Audio API, kein Download nötig)
🔊/🔇-Knopf zum Stummschalten
✕-Button auf jedem Spielbildschirm → zurück zum Menü
Menü mit klickbaren Buttons: Spielen / Bestenliste / Admin

⚙️ Admin-Bereich (PIN: 1234)

Farben ändern: Rakete, Nase, Spielfigur, Monster, Magmamonster
Bestenliste anzeigen, einzelne Einträge löschen, alles löschen
