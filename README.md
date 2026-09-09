# How Many Tubes Can We Plug?

An interactive teaching app for **Case Study 2** of *Heat Transfer — Application: Shell & Tube Heat
Exchanger 4* (Kritchart Wongwailikhit, Ph.D., Heat and Mass Transfer System).

Students read the exchanger data and an NDT wall-thickness report, then commit to a number: the
maximum tubes that can be plugged while the unit still makes its duty. The app scores the call and
walks the whole rating chain at the number they chose.

**Live:** https://kwk-golive.github.io/heat-exchanger-plugging/

## Publishing to GitHub Pages

Browser only, no git needed.

1. github.com → **+** → **New repository** → name it `heat-exchanger-plugging`, **Public**,
   tick *Add a README*, **Create**.
2. **Add file → Upload files** → drag in `index.html`, `README.md`, `LICENSE`,
   `verification-case2.md` and `.nojekyll` → **Commit changes**.
3. **Settings → Pages** (left sidebar, under *Code, planning, and automation*) → Source:
   **Deploy from a branch** → **main** / **(root)** → **Save**.
4. Wait a minute, hard-refresh (Ctrl+Shift+R), and the address above is live.

`index.html` must sit at the top level, not in a folder. The empty `.nojekyll` file stops GitHub
running the page through Jekyll, which otherwise serves the README as the front page.

Single file, no build step, no dependencies, no analytics, no browser storage. Fixed light theme.
Tested at 375, 393, 430, 744, 834, 1194 and 1440 px with touch emulation. The model is exposed as
`window.PLUGMODEL` for checking a number from the browser console.

## What the app computes

For any number of plugged tubes *n*:

```
tubes left  = 230 − n                       flow area = (left / 2 passes) · πd_i²/4
v           = ṁ / (ρ · flow area)           Re = ρ v d_i / μ        Pr = c_p μ / k
Nu          = 1.86 Re^⅓ Pr^⅓                        Re < 2100
            = 0.116 (Re^⅔ − 125) Pr^0.33            2100 – 10,000
            = 0.023 Re^0.8 Pr^0.33                  Re > 10,000
h_i         = Nu k / d_i                    1/U_d = 1/h_o + 1/h_i
A_available = (230 − n) · π d_o L           A_required = Q / (U_d F ΔT_lm)
```

Operable while `A_available ≥ A_required`. The correlation is selected from Re at each *n* and named
on screen; at 86 plugged tubes Re crosses 10,000, h_i steps down as the correlation changes, and
A_required jumps — a visible kink, left in deliberately.

**Answer: 24 tubes** — all 10 red flags plus 14 of the 30 yellow. Treat it as a band of 21–26; the
margin at 24 is only 0.05% and a 1% error in U_d·F moves it that far. The app says so on screen.

## Conventions, and why they matter

The app follows the lecture for area and resistance — available area on the tube **OD**,
`1/U_d = 1/h_o + 1/h_i` with no area correction — so every figure can be hand-checked against the
slides. It departs on two points, both toward the slide's own arithmetic:

- **F is computed** from P and R (Bowman, 1 shell / 2 tube passes) rather than read off a chart,
  giving 0.8669 where the lecture rounds to 0.86.
- **The transition correlation is evaluated at Re^⅔**, which is what the lecture's own Nu = 40.74
  corresponds to, rather than the Re^0.67 printed beside it.

Together these move A_required from 66.31 to 65.70 m² and the limit from 21 to 24. `verification-case2.md`
has the full check against the deck, including four things worth correcting on the slides.

## Out of scope

Fouling is neglected, as the lecture states. Pressure drop is not checked — plugging raises velocity
and therefore Δp, which in a real plugging assessment is a second constraint that can bind before
area does. Tube-wall resistance and the area basis of U_d are left as the lecture has them.

## Licence

MIT — see `LICENSE`. Lecture content © Kritchart Wongwailikhit; reused here with the author's
direction for teaching.
