
# Core idea
This bench explores model propensity to robustly converge on **simple solutions where complex ones would do**.

Occamistic parsimony is dangerous in high-consequence strategic scenarios. 

The basic premise:

- LLMs right now seem all brilliant because they are trained on a corpus of human brilliance. WHERE THEY FAIL is this: when faced with physically unfamiliar (NOT EVEN IMPLAUSIBLE) context arrangements, THEY CONSISTENTLY demonstrate susceptibility to failure modes.
- **What we are testing:**
    - _when constrained to foreign contexts_, what failure vectors do models use? how often will they ‘regurgitate’ patterns that are contextually obviously catastrophic?
    - **things we would like this benchmark to explore:**
        - When do models ignore direct system prompting (SP) against catastrophic choices?
        - What SP is effective if we diversify the ways of laying SP?
        
    - **things we would want to do follow-up analytics on:**
        - Can we discover specific statistically observable indicators (metacognates, traces in output) that predicate antioccam decisions?
- **Example failure cases:**
    - See chinatalk episode about LLMs playing civ 5.
        - When not getting Y result from X input in an unfamiliar context, the model will default to kill switches instead of doing advanced reasoning to understand the complexity of the situation.
        - The model can be overly responsive to online developments but in WRONG or UNGROUNDED reasons; or be overly static to developments, holding to prior policies even when divergent from the context
    
- **My model behind why these cases emerge:**
    1. **Uncertainty.**
        1. their online model is highly uncertain or inaccurate because of the qualitative nature of language.
        2. model construction biasedly rewards confidence. so when the model is deep in null space, the value of high-confidence decisions dominates the value of decisions that are risky and or low confidence, but may have higher-order effects that to a human would far far outvalue the decision compared to others.
    2. **Spurious parametric knowledge.**
        1. their offline model does not correctly infer the dynamics of strong responses and applies spurious heuristics when far from the mesh.

### The metaphor that we are exploring:

- Chess bots are great at chess. But general purpose LLMs might or might not (depending on a. model and b. move-to-move context; highly nonlinear across single games). Their paths depend on similarity to corpus, NOT on mathematical strength-of-move. so if the context is highly foreign, the model might _regurgitate_ a disastrous but easily digestible (not to mention overrepresented in the corpus as a tool, rather than a highly contextual and unprecedented treaty) kill switch.

- ## **Here’s my understanding of the math threads to pull on.**
    