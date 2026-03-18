# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- [x] **Game purpose:** A number guessing game where the player tries to guess a secret number within a limited number of attempts, with hints and a score tracker.
- [x] **Bugs found:**
  - Game locked after a loss — `status` was never reset on new game
  - Hint messages were swapped — "Go HIGHER!" when guess was too high
  - `attempts` initialized to `1` instead of `0` (off-by-one on first guess)
  - Info message and new game both hardcoded range as 1–100, ignoring difficulty
  - Secret cast to string on even attempts, making those guesses unwinnable
  - `update_score` awarded +5 points for wrong guesses on even attempts
- [x] **Fixes applied:** Reset `status`, `history`, and `attempts` on new game; corrected hint messages in `check_guess`; fixed attempt counter initialization; used `low`/`high` from difficulty setting throughout; removed string coercion of secret; removed the even-attempt score bonus for wrong guesses. Core logic was also refactored into `logic_utils.py` and covered with pytest tests.

## 📸 Demo

- [ ] [<img width="1250" height="815" alt="image" src="https://github.com/user-attachments/assets/f5d3f2fe-0155-4158-aa2e-81da1af0cc72" />
]

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, insert a screenshot of your Enhanced Game UI here]
