# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

--- I put in a guess and it told me to guess a number higher and the number of guesses went down
It would tell me to go higher even though I was over the limit of 100. It will also tell me to go lower even though I was guessing below zero.
After reaching the limit of 7 guesses it doesn't reset when you try to start a new game

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

--- I used claude 
When a game ends (win or lose), st.session_state.status gets set to "won" or "lost". When you click New Game, the handler only resets attempts and secret — it never resets status back to "playing". 

It was correct
Claude correctly identified that the new_game handler was missing st.session_state.status = "playing", which caused the game to lock after a loss I verified this by clicking New Game after losing and confirming the game resumed normally instead of immediately hitting st.stop(). Claude also correctly flagged that the hint messages in check_guess were swapped, which I verified through pytest: test_too_high_message_says_go_lower failed before the fix and passed after. On the misleading side, Claude's initial explanation of the "Attempts left: 1" mismatch framed it as a rendering-order bug that needed the info block moved — while the fix worked, it didn't fully eliminate the issue on the same run where the game ends. I noticed this by manually playing through to the last guess and watching both messages appear simultaneously, which showed the fix was partial rather than complete.
## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---
I decided a bug was really fixed by running pytest and confirming the tests passed specifically test_too_high_message_says_go_lower and test_too_low_message_says_go_higher, which targeted the swapped hint messages in check_guess. Before the fix, a guess of 80 when the secret was 50 would tell you to "Go HIGHER!" which is wrong; after the fix, pytest confirmed it returned "Go LOWER!" as expected. Claude both identified the bugs and generated the targeted tests, explaining that the existing tests only checked the outcome string like "Too High" but never asserted what the message said which is exactly where the bug was hiding. That distinction helped me understand that tests need to check the full return value, not just part of it.

For each bug, I verified the fix either through pytest or by manually running the game. The swapped hint messages were confirmed by running py -m pytest tests/test_game_logic.py -v — all 5 tests passed after the fix. The new_game reset bug was verified by playing to a loss, clicking New Game, and confirming the game restarted instead of locking. The hardcoded range and difficulty bugs were checked by switching to Easy mode and confirming the secret stayed within 1–20 and the info message updated correctly. The attempts off-by-one was confirmed through the debug expander, which showed attempt #1 on the first guess instead of #2.

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

---
Every time you click a button or interact with anything in a Streamlit app, the entire Python script runs again from top to bottom. That means any regular variable you create just gets reset to its starting value on every rerun, so if you want something to stick around (like the secret number or the attempt count), you have to store it in st.session_state, which is like a small dictionary that survives between reruns. One bug in this project was that the game status ("playing", "won", "lost") was stored in session state correctly, but the new game button forgot to reset it so even though the script re-ran fresh, the old "lost" status was still sitting in session state and immediately stopped the game again.

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

---
One habit I want to keep is writing tests that check the full return value of a function, not just part of it the existing tests only checked the outcome string and completely missed the broken hint message. Next time I work with AI on a coding task, I would ask it to explain *why* a fix works before accepting it, not just what to change, because the "Attempts left" fix felt correct but I didn't fully understand the Streamlit rendering model until I saw the bug still partially appear on the final guess. This project changed how I think about AI-generated code I used to assume it was either fully right or obviously broken, but here several bugs were subtle and looked reasonable at first glance, which reminded me that AI code still needs to be read critically and tested the same way any other code would be.

