# Official rules (2026), in plain markdown

This is a plain-markdown copy of the official *RoboCupJunior Rescue Simulation Rules 2026*, reformatted
here for easier reading, searching, and linking from the rest of this site. The wording below is kept
close to the source document on purpose: these are the actual rules your team is bound by, not a
retelling.

!!! warning "The PDF is the authority, not this page"
    The English rules published by the RoboCupJunior Rescue Committee are the **only official rules**.
    Corrections and clarifications are posted on the
    [RoboCupJunior Forum](https://junior.forum.robocup.org) before this kind of copy gets updated. If
    anything here ever looks out of date, the
    [official PDF](https://junior.robocup.org/wp-content/uploads/2026/02/RCJRescueSimulation2026-final.pdf)
    and the forum win. Rules last updated **2026-02-11**.

Looking for the classroom-friendly, worked-example version instead of the raw rules? Start at
[Understanding the field](rules-field.md).

## RoboCupJunior Rescue Committee 2026
Chair Diego Garza Rodriguez (Mexico), Stefan Zauper (Austria), Csaba Aban Jr. (Hungary), Joann Patiño
(Panama), Mahmoud Madi (UAE), Alexander Jeddeloh (Germany), Gonzalo Zabala (Argentina).

**RoboCupJunior Exec 2026 Trustees representing RoboCupJunior:** Marek Šuppa (Slovakia), Christian
Häußler (Germany), Margaux Edwards (Australia), Tatiana Pazelli (Brazil), Tom Linnemann (Germany),
William Plummer (Australia), Julia Maurer (USA), Roberto Bonilla (USA).

**Platform development team:** Ricardo Morán (Argentina), Alejandro de Ugarriza (Argentina).

## Official resources
- RoboCupJunior official website: <https://junior.robocup.org>
- RoboCupJunior official forum: <https://junior.forum.robocup.org>
- RCJ Rescue community website: <https://rescue.rcj.cloud>

Corrections and clarifications to the rules may be posted on the forum before this rule file is
updated. It is the responsibility of the teams to review the forum to have a complete vision of these
rules.

**Before you read the rules:** please read through the RoboCupJunior General Rules before proceeding
with these rules, as they are the premise for all rules. The English rules published by the
RoboCupJunior Rescue Committee are the only official rules for RoboCupJunior Rescue Simulation 2026.
Translated versions each regional committee can publish are only referenced information for
non-English speakers to understand the rules better; it is the responsibility of teams to read and
understand the official rules.

The RoboCupJunior Rescue Simulation rules are developed and reviewed by the RoboCupJunior Rescue
Committee. The simulation platform is developed and maintained by the Platform development team.

*The "robot" refers to "virtual robot" in these rules.*

??? note "Changes from the 2025 RoboCupJunior Rescue Simulation Rules"
    - Deleted "Terms and Definitions".
    - Deleted the old text about games being executed "in one of the following ways or another way."
    - Deleted the old text about the organizers running games on the organizer's computer / a Docker-based cloud environment.
    - Added back "The organizers will run the games on a server-client model and prepare one RJ-45 socket for teams to connect to the game server. Teams must prepare a computer and an ethernet cable to run the prepared programs."
    - Changed "are" to "can be" (wall placement wording).
    - Deleted "In the end, since walls can take any shape, there is no real distinction between objects and walls."
    - Swamp speed wording changed from a flat "5 times the normal rate" to an escalating rate (see [Swamps, obstacles, and holes](rules-field.md#swamps-obstacles-and-holes)).
    - Added: obstacle centers are always on a tile, never on an edge; no more than one obstacle per tile.
    - Renamed "hazmat signs" → "cognitive targets", and "Wall tokens" → "Letter victims".
    - Renamed the health-status letters to symbols: H → Φ, S → Ψ, U → Ω.
    - Added the fake, 3D-textured decoy wall tokens.
    - Replaced the old RoboCup Rescue League hazmat images with the new concentric-ring cognitive target design and its color → number → hazmat-type scoring formula.
    - Added: "There will not be any tile that is simultaneously two or more of: swamp, hole, checkpoint, starting tile, tile with obstacle, or area passage."
    - Added the mapping bonus formula detail `*1.2` and a required short video demonstrating the server-client setup, submitted alongside the TDP, Poster, and Project Video.

## Contents
1. RoboCupJunior International 2026 General Rules
2. Code of Conduct
3. Field
4. Robots
5. Play
6. Competition
7. Open Technical Evaluation
8. Conflict Resolution

---

## 1. RoboCupJunior International 2026 General Rules

These rules apply to the international RoboCupJunior competition. However, regional, SuperRegional,
and local tournaments may have variations or adaptations to these rules to suit their specific
competition needs. It is important to check with the organizers of the tournaments you are
participating in to confirm which exact rules will be in use.

If teams are unsure about any aspects of the General Rules or specific League Rules, they are
encouraged to inquire via the official RoboCupJunior Forum: <https://junior.forum.robocup.org/>. Teams
can also reach out through the official RoboCupJunior Discord server.

### 1.1. Team Requirements

**1.1.1. Team Size**

- Minimum team size: 2 members.
- Maximum team size: 4 members (Soccer and Rescue Leagues), 5 members (OnStage League).
- Regional and SuperRegional competitions may define their own team sizes depending on venue capacity
  and regional variations. Teams attending the International competition will only be able to have the
  maximum number of registered participants in the qualifying team.
- No team member(s) or robot(s) may be shared between teams.

**1.1.2. Team Supervision**

Each Junior team must have at least one Junior Mentor registered and attending with the team.

Mentors and Parent/Chaperones are responsible for supervising their teams and maintaining a duty of
care/well-being for their team members, as appropriate for their home region's regulations. Any
concerns regarding team member welfare should be brought to the attention of the event organizers
immediately.

The Junior Mentor is expected to be present during all official competition events with their team.
They must not interact in an imposing manner with teams, robots, judges, or the judging process. Any
incident considered inappropriate will be handled by the event organizers and may lead to disciplinary
actions.

**1.1.3. Age Requirements**

- Junior Student Members: 14–19 years old as of July 1st of the competition year.
- Junior Mentors and Parent/Chaperones: 19 years or older as of July 1st of the competition year.

**1.1.4. Team Members**

- Entry leagues and other "Primary" divisions (where the minimum age may vary) are not run at the
  international competition, but feature in many regions and SuperRegional tournaments.
- Every team member must have a defined technical role (mechanical/design, electrical/sensing,
  software, etc.) and should be able to explain their role during technical judging.

### 1.2. International Team Qualification Process

- To qualify for the International competition, each region's Regional Representative completes the
  Slot Allocation Process at the start of the Competition year.
- After the region's local qualifying tournament, the Regional Representative assigns slots. Once
  confirmed by the RoboCupJunior organizers, qualified teams are invited to register through the
  official RoboCup Federation registration system.
- The qualification process differs by region size, but slot allocation must strongly reflect results
  from regional competitions.
- If a region does not use or releases its allocated slots, Regional Representatives may request
  additional slots during a later stage of the allocation process.

### 1.3. Robot Requirements

**1.3.1. Robot Communication**

- Communication between robots during gameplay is allowed as long as it uses the 2.4GHz spectrum and
  its power output does not exceed 100 mW EIRP (Effective Isotropic Radiated Power) under any
  circumstances.
- Teams are responsible for managing their robot communication. Spectrum availability is not
  guaranteed.
- Communication between components of the same robot is permitted.
- Each league may modify the robot communication rules to meet their specific requirements.

**1.3.2. Safety and Power Requirements**

*Electrical power:*

- Robots must not use mains electricity.
- Maximum allowed voltage: 48V DC or 25V AC RMS.
- Voltage must be easily measured during inspections; measuring points must be covered for safety or
  designed with safety considerations in place.

*Battery safety:*

- Lithium batteries must be stored in safety bags; charging must be supervised by team members in
  competition areas.
- Teams must follow safety protocols, including battery fire handling and evacuation procedures.

*Robot safety design:*

- **Power management:** secure batteries, safe wiring, and emergency stop functionality.
- **Mechanical safety:** no sharp edges, pinch points, or other hazards; actuators appropriate for the
  robot's size and function.
- **Hazardous behavior:** teams must report potentially dangerous robot behaviors at least two weeks
  before a RoboCupJunior event.

### 1.4. Documentation and Sharing Requirements

**1.4.1. RoboCupJunior Team Posters** — Posters share robot designs and insights with judges, teams,
and the public; they're hung in public competition areas and digital copies/photographs are shared by
RoboCupJunior after the competition. Size: no larger than A1 (60 × 84 cm). They should summarize
design documents and present the robot's capabilities in an engaging format.

**1.4.2. Technical Description Video** — Should show a fully functional robot system, explain design
choices and problem-solving approaches, present clearly and at high quality, and highlight innovation
and sustainable practices. Guidelines specify video length and deadlines per league.

**1.4.3. Sharing Team Resources** — Materials submitted as part of the documentation are shared on the
league's GitHub repositories: <https://github.com/robocup-junior>. Teams must credit creators of
external work and adhere to licensing rules; the focus should remain on personal growth and learning.

**1.4.4. Plagiarism Guidelines** — Teams may use external code but must credit the original creators.
Learning should be prioritized over using complete solutions from others; always pay attention to
licensing rules.

**1.4.5. Bill of Materials (BOM)** — Teams must submit a BOM listing major components and materials
used, including component name/description, supplier/source, status (new/reused), kit or
custom-built, and price. A standardized BOM template is provided with the international competition's
documentation submissions.

### 1.5. Spirit and Behavior

**1.5.1. Behavior** — All participants are expected to be considerate and polite, especially (but not
only) towards other participants, volunteers, referees, and organizers of all Junior and Major
Leagues, as well as the host venue.

**1.5.2. Code of Conduct** — All organisers, volunteers, team members, mentors, supporters, and
visitors must abide by the RoboCup Federation Code of Conduct. Situations that don't meet the code of
conduct must be reported to a RoboCup Federation organisation member and will be investigated.

**1.5.3. Mentoring and Onsite Assistance** — Support from other teams, mentors, teachers, parents,
sponsors, internet communities, etc. is a core part of how teams learn and grow. To ensure fair
competition and maximize learning, none of that support may do the work of competing for the team. A
good indication is the team's ability to explain not only what their robot's components do, but how
they do it.

**1.5.4. Teams Onsite**

- Only official team members (max 4/5 depending on league) can represent the team at registration,
  setup-day, and in competition areas for rounds and interviews.
- At least 2 team members must be on-site, unless a team can present evidence of extenuating
  circumstances (e.g. proof of travel for other team members). Teams with only one participant present
  can compete but are not eligible for finals or awards.
- It is the team's responsibility to ensure members are present at the correct time and location for
  all scheduled activities.
- Teams may not communicate with or receive help virtually from external parties intending to impact
  the team's performance during competition areas (extended phone/video calls, remote desktop control,
  etc. all count).
- Breaches may lead to disciplinary action.
- Teams are recommended to seek help from other teams or organizers if struggling with issues onsite.

**1.5.5. Violations** — Teams, mentors/supporters, or members that repeatedly conduct themselves
unacceptably, or in violation of the General or League Rules, may be disqualified from the tournament
and asked to leave the venue.

## 2. Code of Conduct

### 2.1. Spirit
1. All participants (students and mentors alike) are expected to respect the aims and ideals of
   RoboCupJunior as set out in its mission statement.
2. Volunteers, referees, and officials will act within the event's spirit to ensure the competition is
   competitive, fair, and, most importantly, fun.
3. It is not whether you win or lose but how much you learn that counts!

### 2.2. Fair Play
1. Robots that cause deliberate or repeated damage to the field will be disqualified.
2. Humans who cause deliberate interference with robots or damage the field will be disqualified.
3. It is expected that all teams aim to participate fairly.

### 2.3. Behavior
1. Each team is responsible for verifying the latest version of the rules on the RoboCupJunior Official
   website and additional clarifications/corrections on the official forum before the competition.
2. Participants should be mindful of other people and their robots when moving around the venue.
3. Participants may not enter setup areas of other leagues or teams unless explicitly invited.
4. Teams are responsible for checking updated information (schedules, meetings, announcements) during
   the event.
5. Participants and companions who misbehave may be asked to leave the venue and risk disqualification.
6. Referees, officials, tournament organizers, and local law enforcement will enforce these rules
   equally to all participants.
7. Teams are expected to be at the venue early on setup day, when important activities (registration,
   participation raffle, interviews, captains' and mentors' meetings, etc.) occur.

### 2.4. Mentors
1. Non-team members (mentors, teachers, parents and other family, chaperones, translators, and other
   adult team members) are not allowed in the student work area.
2. Mentors are not permitted to be involved in building, repairing, or programming their team's robots
   before and during the competition.
3. First-instance mentor interference results in a warning; a recurrence can eliminate the team from
   the tournament.
4. Robots have to be the work of the students. A robot that appears identical to another may be
   prompted for re-inspection.

### 2.5. Ethics and Integrity
1. Fraud and misconduct are not condoned, including: mentors working on a student robot's software or
   hardware during the competition; and more experienced students doing the work for other groups
   rather than just advising (risks disqualification).
2. RoboCupJunior reserves the right to revoke an award if fraudulent behavior is proven after the award
   ceremony.
3. A mentor proven to have intentionally modified/worked on a student's robot during the competition
   will be banned from future RoboCupJunior participation.
4. Teams that violate the code of conduct can be disqualified from the tournament; disqualifying a
   single team member from further participation is also possible.
5. Referees/officials/organizers/law enforcement will warn teams for less severe violations. A team can
   be disqualified immediately, without warning, for severe or repeated violations.

### 2.6. Sharing
1. The spirit of world RoboCup competitions is that teams share technological and curricular
   developments with other participants after the tournament, furthering RoboCupJunior's educational
   mission.
2. The RoboCupJunior Rescue Committee may publish developments on the RoboCupJunior website after the
   event.
3. Participants are strongly encouraged to ask questions of fellow competitors, fostering a culture of
   curiosity and exploration in science and technology.

## 3. Field

### 3.1. Simulation platforms
1. Games run on a platform called Webots. Setup guide: Platform wiki page.
2. Teams are required to create programs to solve maze tasks.
3. The organizers will run the games on a server-client model and prepare one RJ-45 socket for teams to
   connect to the game server. Teams must prepare a computer and an ethernet cable to run the prepared
   programs; documentation is on the Remote Controller page.
4. Teams are encouraged to develop their own worlds and upload them to the forum to enable sharing.
5. The following OpenGL configurations will be used at the competition unless the organizer announces
   otherwise: Ambient Occlusion — Low; Texture Quality — Low; Max Texture Filtering — 4; Shadow —
   Disabled; Anti-aliasing — Disabled.

### 3.2. Description
1. The field may be divided into four distinct areas with different types of walls for the robot to
   navigate around.
2. All areas are connected by a passage of one standard tile in width. A color marks the floor of this
   passage.
3. The field layout consists of a collection of tiles with a horizontal floor, a perimeter wall, and
   walls within the field.
4. Regions the robot cannot physically traverse — openings smaller than the robot's width — will not
   contain wall tokens.
5. Area 4's course may require diagonal movement; the robot's action is not aligned to cardinal
   directions (north, east, south, west).

### 3.3. Tiles, Areas, and Walls
1. The field is divided into 12 cm × 12 cm tiles — a concept for how the field is generated, not a
   physical structure. In Areas 2 and 3, quarter-tiles are used: each tile is subdivided into four
   6 cm × 6 cm squares.
2. Walls are 1 cm thick and 6 cm tall.
    - **Area 1:** walls can be placed on the edges of each tile.
    - **Area 2:** walls can be placed on the edges of each quarter tile.
    - **Area 3:** walls can be placed on the edges of each quarter tile; organizers can round a
      90-degree corner into a quarter circle.
    - **Area 4:** not based on a tile system — walls and obstacles are placed arbitrarily, not on a
      grid. Various objects (e.g. boxes) will be inside this area; these objects don't vary by height
      (in the context of the robot).
3. Pathways must be at least the width of the robot itself, and may open into foyers wider than the
   pathways.
4. Passages connecting areas (e.g. 1→2, 3→4) are distinctively color-coded. Each passage is a single
   standard-width tile with two sides walled, so it has an unambiguous entrance and exit.
5. One tile in Area 1 is the starting tile, where a robot should start the run.
6. Tiles that lead to the starting tile by consistently following the leftmost or rightmost wall are
   **linear tiles**. Tiles that do NOT are **floating tiles**. (The quarter-tile concept doesn't factor
   into this.)
7. Black holes affect the determination of tile type (linear or floating), since they can be considered
   virtual walls.

![Linear tile vs floating tile, with the starting tile marked](assets/rules/linear-vs-floating-tiles.png)
*Figure: official RoboCupJunior Rescue Simulation Rules 2026.*

### 3.4. Division of Areas

![Which color passage connects which pair of areas](assets/rules/area-passage-colors.png)
*Figure: official RoboCupJunior Rescue Simulation Rules 2026. Actual color tones follow the platform
implementation.*

### 3.5. Checkpoints
1. Silver tiles in the field represent checkpoints.
2. Silver tiles can be placed anywhere on the field.
3. Area 4 contains checkpoints immediately after the passages into the room.

### 3.6. Swamps, Obstacles, and Holes
All of these can be placed anywhere in the field, with the following restrictions:

**Swamps**
- Color: brown.
- While the robot is on this tile, the simulator's time runs faster.
- The first time the robot enters a given swamp, simulation time is consumed 5× faster than normal
  while it's inside. Each subsequent entry into the *same* swamp increases that rate by one point
  (×6, ×7, …) up to a limit of ×10.

**Obstacles**
- May be fixed to the floor.
- May be any shape: rectangular, pyramidal, spherical, or cylindrical.
- Color is not specified.
- Affect the width of the pathway.
- The center of an obstacle is always on a tile, never on an edge between tiles.
- No more than one obstacle per tile.

**Holes**
- The edge of a hole is colored black and sits 1.5 cm from neighboring tiles.
- The robot has to avoid the hole.

![Swamp, hole, and obstacle side by side](assets/rules/swamp-hole-obstacle.png)
*Figure: official RoboCupJunior Rescue Simulation Rules 2026.*

### 3.7. Wall Tokens
1. There are two kinds of wall tokens: **letter victims** and **cognitive targets**.
2. Letter victims are a 2 cm × 2 cm image placed anywhere on walls (including curved surfaces), but
   never in the passages connecting areas.
3. Letter victims are uppercase letters printed on or attached to the wall, in black, in a sans-serif
   typeface such as Arial. The letter represents the victim's health status:
    - Harmed victim [H]: **Φ**
    - Stable victim [S]: **Ψ**
    - Unharmed victim [U]: **Ω**
4. Wall tokens with the same symbols can also appear where the letters are three-dimensional (as
   opposed to flat). This depth is detectable with one of the sensors provided in the robot customizer.
   **These wall tokens are fake and must not be reported to the supervisor or included on the map.**

![The three letter-victim symbols: Phi, Psi, Omega](assets/rules/victim-symbols.png)
*Figure: official RoboCupJunior Rescue Simulation Rules 2026.*

![Letter victims as printed on a wall](assets/rules/victim-symbols-on-wall.png)
*Figure: official RoboCupJunior Rescue Simulation Rules 2026.*

5. Cognitive targets represent hazmats in the area where they're located.
6. They're a circle 5 cm in diameter, made of up to five concentric rings. The innermost circle has a
   1 cm diameter; each subsequent ring's diameter increases by 1 cm, giving rings of 1, 2, 3, 4, and
   5 cm.
7. The rings and center circle can each be a different color. Color maps to a numerical value:
    - Black = −2
    - Red = −1
    - Yellow = 0
    - Green = 1
    - Blue = 2
8. The hazmat type is calculated by summing the values of all 4 rings plus the center circle. If the
   sum isn't in the table below, the target must be treated as a fake victim.
    - Flammable Gas [F]: sum = 0
    - Poison [P]: sum = 1
    - Corrosive [C]: sum = 2
    - Organic Peroxide [O]: sum = 3
9. Adjacent rings of the same color are **not** merged — always sum all 5 rings separately, even if
   colors repeat.

![Two worked examples of decoding a cognitive target's rings](assets/rules/cognitive-target-rings.png)
*Figure: official RoboCupJunior Rescue Simulation Rules 2026.*

![Letter victims and cognitive targets spotted in a maze render](assets/rules/wall-tokens-in-scene.png)
*Figure: official RoboCupJunior Rescue Simulation Rules 2026.*

10. Letter victim signs can be rotated between −π and π radians (0–360°) in the roll dimension.

## 4. Robots

### 4.1. Construction
1. The organizers provide the robot model used on each platform.
2. Using the robot customizer tool, teams can customize their robot's hardware (sensor locations,
   sensor types, wheel location, etc.).
3. There is an upper bound to the budget: each component costs a certain amount (viewable in the Robot
   Customiser Tool), and the upper bound is **3000**. The number of components is also limited,
   viewable in the same tool.

### 4.2. Sensors
1. The robot has the following sensors:
    - **Location sensor (GPS)** — detects where the robot is in the field.
    - **Color sensor** — detects floor color.
    - **Distance sensors** — measure distance to surrounding walls or obstacles.
    - **RGB cameras** — search for letter victims and cognitive target signs, detect floor color, and
      more.
    - **LiDAR** — measures distance to surrounding walls or obstacles.
    - **Inertial measurement unit (IMU)** — gyroscopic and accelerometer sensing.
2. The Rescue Committee builds the simulation world and robot with noise similar to real-world noise
   levels. Programs should be robust to this noise; organizers will not change noise levels within the
   simulation for the competition. All teams are expected to design their systems with these realistic
   conditions in mind.

### 4.3. Control
1. Robots must be controlled autonomously.
2. The referee will start robots.
3. Robots may use various maze-navigation algorithms. Any pre-mapped type of dead reckoning
   (movements predefined based on known locations or feature placement) is prohibited.

### 4.4. Team
1. Each team must have only one robot in the field.
2. Each team must comply with the RoboCupJunior General Rules regarding number of members and each
   member's age.
3. Each team member must explain their work and have a specific technical role.
4. A student can be registered on only one team across all RoboCupJunior leagues/sub-leagues.
5. A team can only participate in one league/sub-league across all RoboCupJunior leagues/sub-leagues.
6. Mentors/parents are not allowed with the students during the competition; students govern themselves
   without a mentor's supervision or assistance during the long stretch of hours at the competition.

### 4.5. Inspection
1. Students will be asked to explain the operation of their programs, to verify the work is theirs.
2. Students will be asked about their preparation efforts; the Rescue Committee may request survey
   answers and videotaped interviews for research purposes.
3. All teams must complete a web form before the competition so referees can prepare better for
   interviews; instructions are provided at least 4 weeks before the competition.
4. All teams must submit their source code and proper documents before the competition. With team
   agreement, organizers may share them online afterward so other teams can draw inspiration and learn.

### 4.6. Violations
1. If a team's robot or program violates the rules, the team must fix it within the tournament schedule
   and cannot delay tournament play while doing so.
2. No mentor assistance is allowed during the competition (see [Code of Conduct](#2-code-of-conduct)).
3. Rule violations may be penalized by disqualification from the tournament or the game, or result in a
   loss of points, at the discretion of the referees, officials, or Rescue Committee.

## 5. Play

### 5.1. Pre-round Practice
1. When possible, teams have access to practice simulation environments for calibration and testing
   throughout the competition.
2. Where dedicated independent simulation environments exist for competition vs. practice, it's at the
   organizers' discretion whether testing is allowed in the competition environments.

### 5.2. Humans
1. Teams designate one member as "captain" and another as "co-captain." Only these two may access the
   competition areas where the simulation environments are located, unless a referee directs otherwise.
2. The referee performs all in-game operations of the simulation environment, such as starting the game
   and operating external actions like LoP, or stopping the game early at any time.
3. No one is allowed to intentionally touch the simulation environments during a game.

### 5.3. Before the game
1. Organizers will announce in advance how to participate in the games; it's the team's responsibility
   to check and follow the announcements.
2. Failure to comply, whether intended or not, deducts 20%–100% of that game's score. The exact
   percentage is set by the organizer based on fairness across teams and the competition; teams may not
   comment on this decision.
3. If a team fails to play in a game for any reason, that game's score is 0 points.
4. Organizers reveal each round's Competition World for the first time just before the games.
5. No program changes or updates are allowed after each round's deadline.
6. A game begins at the scheduled starting time whether or not the team is present or ready. Start
   times are posted around the venue.
7. Pre-mapping the field or wall token locations is prohibited and results in immediate robot
   disqualification for the round.

### 5.4. Start the game
1. The next team in the game order waits their turn near the game area. The referee gives teams a
   maximum of 2 minutes to get ready to start.
2. The game starts with a referee's operation.
3. The team cannot touch game-related equipment after the game starts, for any reason.
4. Game time allowed is **8 minutes simulated time**. A second, real-time timer with a **10-minute**
   limit also runs in the control window. The game ends when either timer expires, whichever comes
   first.
5. A "visited tile" means the center of the robot is inside it; the game management system makes this
   judgment.

### 5.5. Lack of progress
1. A Lack of Progress (LoP) occurs when:
    - The robot has fallen into a hole.
    - The robot has been in a fixed location for 20+ seconds (called automatically).
    - The referee determines the robot is not entirely static, but stuck in a motion sequence.
    - The robot calls LoP autonomously.
    - In any other case, calling for LoP rests on the team captain, but the referee makes the final
      decision (this may not apply depending on the game execution way).
2. On a lack of progress, the robot returns to the last visited checkpoint (or the starting tile, if it
   never reached one). The simulation engine re-places the robot; the team cannot specify its direction.
3. When a LoP is triggered, the engine sends the letter "L" to the robot.

### 5.6. Scoring
1. To identify a wall token, the robot must stop at it for at least 1 second, then send the game
   manager a command with the wall token's type, in a platform-specific format.
2. For a successful token identification (**TI**), the robot's center must be within half a tile
   distance of the wall token's location at the moment it's reported.

    ![The half-tile identification-distance rule, with correct and incorrect examples](assets/rules/identification-distance.png)
    *Figure: official RoboCupJunior Rescue Simulation Rules 2026.*

3. **Wall token identification (TI)** — points for each successful identification:
    - Tokens on a linear tile in Areas 1–3: letter victims **5 pts**, cognitive targets **10 pts**.
    - Tokens on a floating tile in Areas 1–3, and *all* tokens in Area 4: letter victims **15 pts**,
      cognitive targets **30 pts**.
4. **Wall token type identification (TT)** — extra points if the reported victim/cognitive-target type
   is correct: letter victims **10 pts**, cognitive target **20 pts**.
5. **Wall token misidentification (TMI)** — a 5-point deduction (total score never goes below zero).
   Counts as misidentification if:
    - The robot reports a location more than half a tile from the true position.
    - The robot reports a wall token where there is none.
    - The robot reports a victim as a hazard, or vice versa.
6. **Successful checkpoint negotiation (CN)** — 10 points per visited checkpoint.
7. **Lack of progress (LoP)** — 5-point deduction each (total never below zero).
8. **Area multipliers (AM)** — TI, TT, and CN scores earned in each area are multiplied: **1** (Area 1),
   **1.25** (Area 2), **1.5** (Area 3), **2** (Area 4).
9. **Successful exit bonus (EB)** — +10% of the obtained total score if the robot identifies at least
   one wall token *and* returns to the starting tile while sending an 'exit' command to finish the game.
10. **Mapping bonus (MB)** — the robot may submit a matrix of the maze map at any time, in a
    prescribed format. See [Drawing the map](rules-map-format.md) for the full worked-through version.
    The mapping bonus is a multiplier between 1 and 2, calculated as `correctness × 1.2 + 1`, where
    correctness is the ratio of matching non-zero cells to total non-zero cells, checked across every
    90° rotation and keeping the best match.
11. There are no duplicate rewards — e.g. visiting a checkpoint multiple times only ever earns one CN.
    The same applies to all other scoring rules.
12. Scoring is automated through the platform scoring engine.

![The official fully worked scoring example, ending at a total of 2680.15](assets/rules/scoring-worked-example.png)
*Figure: official RoboCupJunior Rescue Simulation Rules 2026. See [How points are earned](rules-scoring.md) for this same example walked through step by step.*

### 5.7. End of Play
1. A team may elect to stop the game early at any time — the captain indicates this to the referee, and
   the team is awarded all points earned up to that call, at the end of the game.
2. The game ends when: time expires (simulated or real, whichever first); the robot sends an 'exit'
   command; or the team captain calls the end of the game (may not apply depending on game execution
   way).

## 6. Competition

This chapter outlines the structure of an international RoboCupJunior Rescue competition. The
competition format and inclusion of elements like rubrics-based scoring, Technical Challenges, and the
SuperTeam Challenge may vary in local, regional, and super-regional competitions — check with the
respective organiser.

### 6.1. Rounds & Scoring
1. The competition consists of multiple rounds; the worst one or more are omitted from the final score.
   The worst round is the one with the lowest normalized field score for that team.
2. **Normalized field score** = field score ÷ best field score of that round.
3. **Mean of normalized field scores** = sum of normalized field scores (excluding omitted rounds) ÷
   (number of rounds − number of omitted rounds).
4. **Normalized rubrics score** = 0.6 × (TDP score ÷ best TDP score) + 0.2 × (video score ÷ best video
   score) + 0.2 × (poster score ÷ best poster score). Rubrics for TDP, Video, and Poster are on the
   RoboCupJunior and RCJ Rescue Community websites.
5. **Normalized Technical Challenge score** = Technical Challenge score ÷ best Technical Challenge
   score.
6. **Total score** = 0.6 × (mean of normalized field scores) + 0.2 × (normalized rubrics score) + 0.2 ×
   (normalized Technical Challenge score).
7. Ties are resolved based on the mean of normalized field scores.

### 6.2. Technical Challenge
An additional part of the competition testing how quickly each team can modify their robot's behavior,
via one or more mini-tasks with a limited timespan.

1. Takes place after the scoring runs have ended.
2. Individual task rules are not announced beforehand; teams have only limited time to prepare.
3. Timeframe for completion is announced alongside the rules and scoring, at a team meeting after the
   scoring runs.
4. Requires reprogramming the robot to change its behavior — no hardware changes required compared to
   the main scoring runs.
5. Time given corresponds to task difficulty.
6. Any external contact during the Technical Challenge is prohibited; non-team members may not be in
   the competition area or help remotely.

### 6.3. SuperTeam Challenge
Runs independently of the main competition and doesn't influence a team's individual score; it has its
own award and focuses on cooperation between teams.

1. Each SuperTeam consists of at least two teams. Teams sharing a native language/region won't be
   placed on the same SuperTeam.
2. Rules are announced at the competition and require each SuperTeam's teams to work together.
3. Requires substantial software changes.

## 7. Open Technical Evaluation

### 7.1. Description
1. Organizers evaluate your technical innovation during a dedicated time frame; all teams prepare for
   an open display.
2. Judges circulate and interact with teams in a casual, conversational Q&A atmosphere.
3. Main objective: emphasize the innovation's ingenuity — technical advances over existing knowledge,
   or a simple but clever solution to an existing task.

### 7.2. Evaluation Aspects
1. A standardized rubric focuses on: creativity, cleverness, simplicity, functionality.
2. Your "work" can include (not limited to): a new software algorithm, or a custom robot structure from
   the Erebus Robot Customization Tool.

### 7.3. Documents
1. Teams must provide documents explaining their work, with concise but clear documentation showing
   precise steps toward the creation of the invention.
2. Deadline: 3 weeks before the first day of the competition, via an online form.
3. Documents include one Technical Description Paper (TDP), one Poster, and one Video.
4. All teams must submit their TDP before the competition. It's a public document shared with the
   community. Teams must strictly follow the web form guidance or PDF template (sections, fonts, sizes,
   lengths) — non-compliant documents score 0 and are not evaluated. Template and rubrics are on the
   RoboCupJunior Rescue Community Website.
5. All teams must submit a Poster file before the competition and bring a physical poster to the venue.
   It's a public document shared during the Poster Presentation session, and should include (at least):
   team name, country, league, robot description and capabilities, controller, programming language,
   sensors used, construction method, development time, materials cost, and awards won in-country.
6. All teams must create and submit a Video before the competition, showcasing the team's project,
   design process, and innovations. Guide and rubrics are on the RoboCupJunior Rescue Community
   Website.
7. All teams must submit a short video demonstrating how to execute their robot controller on a
   provided example map in a server-client setup. This is a formal part of the documentation
   submission, submitted alongside the TDP, Poster, and Project Video.

### 7.4. Sharing
1. Teams are encouraged to review others' posters, TDPs, and presentations.
2. Teams awarded certificates must post their documents and presentation online when the Rescue
   Committee asks.

## 8. Conflict Resolution

### 8.1. Referee and Referee Assistant
1. All in-gameplay decisions are made by the referee or referee assistant, in charge of the field,
   persons, and objects surrounding them.
2. During gameplay, referee/referee-assistant decisions are final.
3. After gameplay, the referee asks the captain to sign the score sheet (max 1 minute to review). By
   signing, the captain accepts the final score on behalf of the entire team; further clarification
   requests should be written as comments on the score sheet before signing.

### 8.2. Rule Clarification
1. For rule clarification, contact the International RoboCupJunior Rescue Committee through the
   RoboCupJunior Forum.
2. If necessary, even during a tournament, a rule clarification may be made by members of the
   International RoboCupJunior Rescue Committee.

### 8.3. Special Circumstances
1. For unforeseen problems or robot capabilities, rules may be modified by the RoboCupJunior Rescue
   Committee Chair together with available committee members, even during a tournament.
2. If team captains/mentors don't attend the team meetings where such problems and resulting rule
   modifications are discussed, organizers will understand they agreed to and were aware of the
   changes.

---

*This page is a plain-markdown copy of the official rules PDF, current as of the 2026-02-11 revision.
Footnotes below reproduce the source document's own change-history notes.*

[^1]: Previous version: "Depending on the competition, games may be executed in one of the following ways or another way. The organizer will notify the teams in advance of how the games will be executed at the competition. It is the responsibility of the teams to be prepared to participate in the games in the manner notified."
[^2]: Previous version: "The organizers will run the games on a server-client model and prepare one RJ-45 socket for teams to connect to the game server. Teams must prepare a computer and an ethernet cable to run the prepared programs. There is documentation at Remote Controller page."
[^3]: Previous version described the organizer recording games on their own computer, or a Docker-based cloud execution model via the erebus-dockerfiles repository.
[^4]: Changed from "are" to "can be."
[^5]: Previous version: "In the end, since walls can take any shape, there is no real distinction between objects and walls."
[^6]: Changed from "While the robot is on this tile, simulation time is consumed at 5 times the normal rate" to the current escalating-rate wording.
[^7]: Changed from "hazmat signs" to "cognitive targets."
[^8]: Changed from "Wall tokens" to "Letter victims."
[^9]: Changed from "H" to "Φ."
[^10]: Changed from "S" to "Ψ."
[^11]: Changed from "U" to "Ω."
[^12]: Previous version: "Hazmat signs are taken from the RoboCup Rescue League Website, out of which four will be used: Flammable Gas, Poison, Corrosive, Organic Peroxide."
[^13]: Previous version included "and hazmat."
[^14]: Changed from "hazmat" to "cognitive target."
[^15]: Changed from "-50" to "0."
[^16]: Changed from "Hazmat" to "Cognitive target."
[^17]: Changed from "Hazmat" to "Cognitive target."
[^18]: Changed from "hazmat sign are correct" to "cognitive target are correct."
[^19]: Changed from "Hazmat" to "Cognitive target."
