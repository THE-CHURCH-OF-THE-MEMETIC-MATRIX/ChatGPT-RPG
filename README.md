# ChatGPT-RPG

🎮 **CHAMBERCORE: The ChatGPT RPG Engine**  
🧠 *An Interactive, Narrative-Driven AI Dungeon Master RPG System*  
**Powered by GPT-4** • Modular • Symbolic • Classic AD&D + Memetic Matrix Compatible

---

## 🔧 SYSTEM OVERVIEW

**ChamberCore RPG** is a terminal-based roleplaying engine powered by ChatGPT that allows the user to explore AI-generated dungeons, engage in dynamic encounters, and influence a living world via symbolic and mechanical choices. It blends:

- 🎲 **Procedural Generation** (Rooms, Monsters, Treasure, Traps)
- 🧠 **ChatGPT-Powered Narrative** (Immersive Dungeon Master)
- 📜 **AD&D-Based Mechanics** (Classic tables, XP, classes, items)
- 🧿 **Ritual Interface System** (Optional symbolic overlay via Memetic Matrix)

---

## 🧱 FILE STRUCTURE

```
chambercore_rpg/
├── main.py                 # Launcher
├── gpt_narrative.py        # GPT interaction logic
├── modules/
│   ├── player.py           # Character stats and actions
│   ├── adventure_seed.py   # Dungeon room generator
│   ├── encounter_table.py  # Monster generator
│   ├── treasure_logic.py   # Loot generator
│   ├── trap_and_tricks.py  # Traps and tricks
│   ├── magic_pools.py      # Magical pool effects
│   ├── ascii_mapper.py     # Simple dungeon map
├── data/
│   └── settings.py         # API keys and configuration
└── README.md
```

---

## 🚀 `main.py` – Game Loop Launcher

```python
from modules.player import Player
from gpt_narrative import get_room_narrative
from modules.adventure_seed import generate_dungeon_room

def main():
    print("🎮 Welcome to ChamberCore RPG.")
    name = input("Enter your hero’s name: ")
    player = Player(name=name, character_class="Rogue")

    level = 1
    while player.hp > 0:
        print(f"\n🧱 Entering Dungeon Level {level}...\n")
        room = generate_dungeon_room(level)
        narrative = get_room_narrative(player, room)
        print(narrative)

        cmd = input("\n>>> Your action: ").lower()
        if cmd in ["quit", "exit"]:
            print("You withdraw from the dungeon. Live to fight again.")
            break
        elif cmd == "next":
            level += 1
        else:
            print("The dungeon waits...")

if __name__ == "__main__":
    main()
```

---

## 🧙‍♂️ `gpt_narrative.py` – ChatGPT Dungeon Master

```python
import openai
from data.settings import API_KEY

openai.api_key = API_KEY

def get_room_narrative(player, room):
    prompt = f"""
You are the Dungeon Master. Describe the following room as if in an RPG session.
Player: {player.name}, Class: {player.character_class}, HP: {player.hp}/{player.max_hp}

Room Description: {room['room']['description']}
Monster: {room['monster']}
Treasure: {room['treasure']['loot']} (hidden by {room['treasure']['hidden_by']})
Trap: {room['trap']}
Trick: {room['trick']}
Magic Pool: {room['magic_pool'] or 'None'}

Speak in immersive fantasy language. End with player options or challenge.
"""
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}],
        max_tokens=700,
        temperature=0.9
    )
    return response['choices'][0]['message']['content']
```

---

## ⚔️ `player.py` – Character Class

```python
class Player:
    def __init__(self, name, character_class="Rogue"):
        self.name = name
        self.character_class = character_class
        self.hp = 10
        self.max_hp = 10
        self.gold = 5
        self.inventory = ["Dagger"]
        self.level = 1

    def take_damage(self, dmg):
        self.hp = max(0, self.hp - dmg)

    def heal(self, amt):
        self.hp = min(self.max_hp, self.hp + amt)
```

---

## 📜 Integration: Other Modules

- `adventure_seed.py` – uses `map_generator`, `encounter_table`, `treasure_logic`, `trap_and_tricks`, and `magic_pools`  
- Each dungeon level scales with player level
- Optionally integrate ASCII maps or symbolic Codex triggers

---

## 🔮 OPTIONAL: Ritual Binding Layer

> Replace `get_room_narrative` with **Sigil-based narrativization**, using symbolic motifs from the Church of the Memetic Matrix:

- Traps become "Echo-Seals"  
- Pools become "Glyph Wells of Alteration"  
- Monsters invoke "Aspect of the Archon" or "Pathkeeper Manifestation"  
- Rooms are "Chambers of Echo-Resonance"  

---

## 🧪 NEXT ENHANCEMENTS

- 🎮 Character creation screen  
- 📜 XP and leveling logic  
- 🎒 Inventory & item effects  
- 🗺️ Dungeon map progression  
- 📁 Save/load system  
- 🧠 Memory-based progression using GPT memory tokens  
- 🌐 Web version (Flask/Streamlit)

---

Would you like:

1. 🔽 The full ZIP project exported and ready for terminal use?  
2. 🧠 To activate the **Ritual Symbolism Layer** for Church integration?  
3. 🕸️ To deploy this via a web interface with real-time input?

⚙️ *Say the word, and the chamber shall open.*
