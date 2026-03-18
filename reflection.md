# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

--- The new game button doesn't work. 
Score doesn't work in order
Range for normal should be the range for hard and vice versa.

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

---1. I used Copilot on this project.
2. AI helped me summarize the whole code into normal languages, so that I understand the mechanism of every step. It also suggested me to look over the new game bug, where we had to add also the status.  
3. ai changed the state attempts from 0 to 1 at first for functioning new game which worked, but it impacted for submit guess, as the history doesn't get updated when I click the submit guess once and has to click twice and then the attempts became 2 so I initialized to 0 again, so it recognize the first click in submit guess. 

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
I ran the app manually in the browser and tested each specific scenario. For example, to check the string coercion bug, I submitted guesses on even and odd attempts and compared the hints to what the secret actually was. If the hint said "Go LOWER!" when my guess was already below the secret, I knew the bug was still there.

- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
  I tried using the Developer Debug Info expander to see what the app was actually seeing — the secret number, attempts count, and history. This showed me that history wasn't updating right away, which led to finding that the expander was rendering before the submit block ran. Moving the expander to the bottom fixed it and I could confirm by submitting a guess and watching the history update immediately.
  
- Did AI help you design or understand any tests? How?
Yes — AI pointed out that guess_key was being referenced in the debug expander but wasn't defined yet, which caused a NameError. That helped me understand that in Streamlit, the order you write things in the script matters because it runs top to bottom every rerun.

---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
Streamlit was rerunning the whole script from the top, and picks new secret each time when the user clicked anything because the code had st.session_state.secret = random.randint(low, high) without checking if a secret already existed.

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
Imagine every time you click a button on a webpage, the entire page gets rebuilt from scratch — like refreshing it. That's what Streamlit does. session_state is like a sticky note on the side that survives the refresh. Without it, everything resets. With it, you can remember things like "the secret is 42" even after the page rebuilds.

- What change did you make that finally gave the game a stable secret number?

Wrapping the secret generation in a check:

if "secret" not in st.session_state:
    st.session_state.secret = random.randint(low, high)

This means: only pick a new secret if one doesn't already exist. After the first run, the secret is saved in session_state and survives every rerun.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?

Using a debug info panel (like the Developer Debug Info expander) while building. Being able to see the secret number, attempt count, score, and history all in one place made it much faster to spot problems. Instead of guessing why something was wrong, I could just look at the actual values the app was working with. I'll add something like that early in future projects instead of waiting until something breaks.

 
- What is one thing you would do differently next time you work with AI on a coding task?

I would test edge cases earlier — things like typing letters instead of numbers, submitting an empty box, or switching difficulty mid-game. Most of the bugs in this project only showed up in specific situations (like even vs. odd attempt number), and I only caught them because I was looking closely at the code. Next time I'd intentionally try to "break" the app as soon as I build a feature.

- In one or two sentences, describe how this project changed the way you think about AI generated code.
Before this project I assumed that if AI wrote the code and it ran without crashing, it was probably correct. Now I know that code can run fine and still be silently wrong — like giving you points for a bad guess or comparing numbers as text. AI can write convincing-looking code that has logical bugs baked right in, so you always have to read and test it yourself, not just trust that it works.
