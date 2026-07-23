# Quantum Prisoner's Dilemma — an interactive simulator

**How does entanglement dissolve game theory's most famous trap?**

An exact, interactive simulation of the **Eisert–Wilkens–Lewenstein (EWL)** quantum Prisoner's Dilemma.
Turn the entanglement dial and watch the only rational move flip from *betray* to *cooperate* — passing
through **two** exact tipping points, at **sin²γ = 1/5** and **2/5**.

Built as the companion to my paper, *“How can quantum algorithms provide insight into economic outcomes?”*
([`papers/`](papers/)).

### ▶ [Open the live simulator](https://gitlostinthesauce.github.io/Quantum-Prisoner-s-Dilemma-Simulation/)

*(served from `index.html` via GitHub Pages — no install, runs entirely in the browser)*

---

## What's here

| File | What it is |
|------|-----------|
| [`index.html`](index.html) | **The interactive explorer.** An exact 2-qubit state-vector simulator in vanilla JS — strategy pads for both players, an entanglement slider, live payoffs, a payoff-landscape heatmap, and the γ-sweep showing where the dilemma breaks. |
| [`notebook/quantum_prisoners_dilemma.ipynb`](notebook/quantum_prisoners_dilemma.ipynb) | The same physics on real quantum tooling (**Qiskit + Aer**): builds the EWL circuit with `Ĵ(γ)` and the `Û(θ,φ)` strategies, verifies payoffs exactly, samples on the simulator, and regenerates both figures. |
| [`papers/`](papers/) | My paper, plus the survey I built on (Wang, *Advantages and Applications of Quantum Game Theory*, 2022). |

## The idea in one minute

In the classical Prisoner's Dilemma, whatever the other player does, defecting pays more — so two rational
players both defect and land on **(1, 1)**, even though mutual cooperation pays **(3, 3)**.

|                    | Bob: Cooperate | Bob: Defect |
|--------------------|:--------------:|:-----------:|
| **Alice: Cooperate** | 3, 3         | 0, 5        |
| **Alice: Defect**    | 5, 0         | 1, 1        |

EWL give each player one qubit of a shared **entangled** pair before they choose:

1. Entangle both qubits with a gate **Ĵ(γ)** — `γ` sets how correlated the players are.
2. Each player privately applies a strategy: a rotation **Û(θ, φ)** of their own qubit.
3. Un-entangle with **Ĵ†**, measure, and apply the payoff matrix.

At **γ = 0** this is exactly the classical game (defection dominates). But qubits allow a purely quantum move
**Q̂ = Û(0, π/2)** with no classical analogue. Once entanglement is strong enough, **Q is every player's best
response to Q** — so **(Q, Q) is a new Nash equilibrium paying (3, 3)**, and the dilemma is gone.

## The result — two thresholds, not one

Cooperation doesn't fade in gradually, but it also doesn't switch on at a single magic value. As γ increases,
the game crosses **two** exact thresholds with clean closed forms:

- **γ₁ = arcsin√(1/5) ≈ 0.464** — below it, mutual defection (D,D) is still the Nash equilibrium (the classical dilemma survives);
- **γ₂ = arcsin√(2/5) ≈ 0.685** — above it, mutual quantum cooperation (Q,Q) becomes the Nash equilibrium and pays (3,3).

Between them is a **frustrated region with no pure-strategy equilibrium**. These `sin²γ = 1/5` and `2/5`
boundaries are the standard EWL thresholds — verified here numerically, not original to this project. The
γ-sweep in the simulator (and §5 of the notebook) draws all three regions.

## Why it's more than a physics curiosity

The equilibria of strategic interactions — oligopoly pricing, public-goods problems, international agreements —
aren't fixed. They depend on the correlation structure the players can share. Change what the players are
allowed to share, and you change what "rational" means. Wang (2022) carries the same machinery to Cournot
duopoly, where a regulator tuning entanglement can keep a second firm in the market and prevent a monopoly.

## The physics, precisely

- Entangler: **Ĵ(γ) = exp(i·γ/2·σ_y⊗σ_y)**, maximal at γ = π/2.
- Strategy: **Û(θ, φ) = [[e^{iφ}cos(θ/2), sin(θ/2)], [−sin(θ/2), e^{−iφ}cos(θ/2)]]**, with
  **C = Û(0,0) = I**, **D = Û(π,0)** (bit flip), **Q = Û(0, π/2) = iZ**.
- Payoffs (reward, sucker, temptation, punishment) = **(3, 0, 5, 1)**.

The browser simulator and the Qiskit notebook are independent implementations that agree to numerical
precision; both were checked against a NumPy reference reproducing the canonical EWL results
(Q vs Q → (3,3), D vs D → (1,1), the miracle move Q vs D → (5,0)).

## Run the notebook

```bash
pip install "qiskit>=1.0" "qiskit-aer>=0.14" numpy matplotlib pylatexenc
jupyter notebook notebook/quantum_prisoners_dilemma.ipynb
```

Or open it in [Google Colab](https://colab.research.google.com/github/GitLostintheSauce/Quantum-Prisoner-s-Dilemma-Simulation/blob/main/notebook/quantum_prisoners_dilemma.ipynb).

## References

- J. Eisert, M. Wilkens, M. Lewenstein, *Quantum games and quantum strategies*, Phys. Rev. Lett. **83**, 3077 (1999).
- H. Wang, *Advantages and Applications of Quantum Game Theory* (2022) — [`papers/wang-quantum-game-theory.pdf`](papers/wang-quantum-game-theory.pdf).
- E. Adams, *How can quantum algorithms provide insight into economic outcomes?* — [`papers/adams-quantum-algorithms-economics.pdf`](papers/adams-quantum-algorithms-economics.pdf).
