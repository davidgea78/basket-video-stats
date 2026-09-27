1. Principi fonamental

Tot el sistema girarà al voltant d'una única font de veritat:

Plain Text
EVENTS
Show more lines

Absolutament tot es calcularà a partir dels esdeveniments:

Marcador
Estadístiques
Temps a pista
+/-
EFF
PIR
Dashboard
Overlay
Resum final

Mai guardarem estadístiques calculades.

Sempre les recalcularem a partir dels esdeveniments.

2. Arquitectura general
Plain Text
┌─────────────────┐
│ UI │
└────────┬────────┘
│
▼
┌─────────────────┐
│ Game Engine │
└────────┬────────┘
│
▼
┌─────────────────┐
│ Events │
└────────┬────────┘
│
▼
┌─────────────────┐
│ SQLite / JSON │
└─────────────────┘
``
Show more lines
3. Classes principals
Team
Plain Text
Team
 
Show more lines

Responsabilitat:

Plain Text
Representar un equip.
Show more lines

Propietats:

Plain Text
id
name
short_name
primary_color
secondary_color
Show more lines

Exemple:

JSON
{
"id": 1,
"name": "FCBQ Mini A",
"short_name": "FCBQ"
}
Show more lines
Player
Plain Text
Player
Show more lines

Propietats:

Plain Text
id
team_id
number
name
``
Show more lines

Exemple:

JSON
{
"id": 15,
"team_id": 1,
"number": 15,
"name": "Mara"
}
 
Show more lines
Game
Plain Text
Game
Show more lines

Propietats:

Plain Text
id
date
home_team
away_team
 
periods
 
seconds_per_period
Show more lines

Exemple:

JSON
{
"id": 100,
"periods": 8,
"seconds_per_period": 360
}
Show more lines
Event

És la classe més important.

Plain Text
Event
Show more lines

Propietats:

Plain Text
id
 
game_id
 
period
 
game_clock
 
team_id
 
player_id
 
action
 
notes
Show more lines

Exemple:

JSON
{
"period": 2,
"game_clock": 248,
"player_id": 15,
"action": "2P"
}
Show more lines
4. Enumeració d'accions

Per evitar errors tipogràfics.

Plain Text
2P
3P
FT
 
_2P
_3P
_FT
 
AS
 
OR
DR
 
ST
 
TO
 
FO
ANT
TEC
 
SUB_IN
SUB_OUT
Show more lines
5. Base de dades SQLite
games
Plain Text
id
game_date
 
home_team_id
away_team_id
 
periods
seconds_per_period
Show more lines
teams
Plain Text
id
name
short_name
primary_color
secondary_color
Show more lines
players
Plain Text
id
team_id
 
number
name
Show more lines
events
Plain Text
id
 
game_id
 
period
 
game_clock
 
team_id
 
player_id
 
action
 
notes
Show more lines
6. Fitxer JSON de partit

Cada partit podrà exportar-se.

Exemple:

JSON
{
"game": {
"date": "2026-09-21",
"periods": 8,
"period_duration": 360
},
 
"events": [
{
"period": 1,
"clock": 324,
"player": "Mara",
"action": "2P"
}
]
}
Show more lines

Aquest format serà el futur pont entre:

Plain Text
APP
VIDEO
OVERLAY
EXPORTS
Show more lines
7. Motor d'estadístiques
StatsEngine

Servei encarregat de calcular:

Plain Text
PTS
 
ORB
DRB
TREB
 
AST
 
STL
 
TO
 
FOL
 
2FGM
2FGA
 
3FGM
3FGA
 
FTM
FTA
 
FGM
FGA
Show more lines

Entrada:

Plain Text
Events
 
Show more lines

Sortida:

Plain Text
PlayerStats
Show more lines
8. Runtime Engine
RuntimeEngine

Responsable de calcular:

Plain Text
Temps jugat
 
Show more lines

A partir de:

Plain Text
SUB_IN
 
SUB_OUT
Show more lines

Exemple:

Plain Text
Q1
 
SUB_IN 6:00
 
SUB_OUT 2:35
 
Show more lines

Resultat:

Plain Text
3:25
Show more lines

També calcularà:

Plain Text
Temps per quart
 
Temps total
Show more lines
9. PlusMinus Engine
PlusMinusEngine

Responsable de calcular:

Plain Text
+/-
Show more lines

Lògica:

Si un jugador és a pista quan:

Plain Text
Equip +2
Show more lines

→

Plain Text
+2
Show more lines

Si el rival marca:

Plain Text
+3
Show more lines

→

Plain Text
-3
Show more lines

Resultat:

Plain Text
-1
Show more lines

Necessita:

Plain Text
Active Lineup
Show more lines

en cada instant.

10. State Engine

Nova peça molt important.

GameState

Mantindrà:

Plain Text
Quarter actual
 
Temps actual
 
Marcador local
 
Marcador visitant
 
Jugadors a pista
Show more lines

Exemple:

JSON
{
"period": 4,
"clock": 152,
 
"home_score": 43,
"away_score": 39
}
Show more lines

Aquest objecte serà utilitzat més endavant pel vídeo.

11. Gestió de quintets

Extremadament important.

Crearem:

Plain Text
LineupManager
Show more lines

Responsabilitats:

Plain Text
Validar màxim 5 jugadors
 
Registrar canvis
 
Conèixer quintet actiu
Show more lines

Exemple:

Plain Text
IN
Mara
 
OUT
Nora
Show more lines
12. MVP de la interfície

No vídeo.

No càmera.

Només:

Pantalla 1
Plain Text
Partits
Show more lines

Botons:

Plain Text
Nou partit
 
Obrir partit
Show more lines
Pantalla 2
Plain Text
Configuració partit
Show more lines

Equips.

Jugadors.

Duració quart.

Pantalla 3
Plain Text
Captura esdeveniments
Show more lines

Cronòmetre.

Marcador.

Jugadors.

Botons d'acció.

Pantalla 4
Plain Text
Estadístiques
Show more lines

Resum final.

13. Estructura del projecte
Plain Text
basket-video-stats
 
docs/
 
src/
 
models/
 
game.py
team.py
player.py
event.py
 
services/
 
stats_engine.py
 
runtime_engine.py
 
plusminus_engine.py
 
lineup_manager.py
 
game_state.py
 
storage/
 
database.py
 
json_export.py
 
ui/
 
main.py
 
tests/
Show more lines
14. Primer desenvolupament real

El primer objectiu executable no serà una app mòbil.

Serà:

Plain Text
python main.py
Show more lines

i ha de permetre:

Crear partit.
Crear equips.
Crear jugadors.
Registrar esdeveniments.
Guardar JSON.
Calcular estadístiques.
Calcular temps.
Calcular +/-