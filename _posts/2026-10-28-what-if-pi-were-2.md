---
layout: post
title: "What If π Were 2?"
date: 2026-10-28 11:40:00 +0100
tags:
  - "math"
  - "geometry"
  - "kids"
  - "interactive"
---

One day my kid looked up from a worksheet and asked: "what would circles look like if π wasn't 3.14?" I didn't have a good answer. So I built one.

Here's the equation for a circle: `x² + y² = r²`. Every point, same distance `r` from the center. That's the whole thing. Now look closely — there's no π anywhere in it.

π shows up later, once you ask how big the circle actually is — circumference `C = 2πr`, area `A = πr²`. Those are things you calculate *about* the shape, not something baked into it. So "change π" can't mean editing that equation — there's nothing in there to change. It has to mean something sneakier: build a shape where area ÷ radius² is an actual dial, then crank it to 2 and see what falls out.

Turns out mathematicians already built that dial — they just call it **p**, the exponent in a p-norm, math's general-purpose way to measure distance. Ordinary straight-line distance, the kind circles are made of, is `p = 2`. Taxicab distance — city blocks, no cutting corners — is `p = 1`. Pick any `p` and you get your own flavor of distance, and your own flavor of "circle." Sorry, Euclid, you don't own the word.

Drop that exponent into the circle's equation:

```
|x|^p + |y|^p = r^p
```

Plug in `p = 2` and you get the circle you already know. Dial `p` down toward 1 and the curve straightens, corner by corner, into a diamond. At `p = 1` — tips sitting at `(±r, 0)` and `(0, ±r)` — that diamond's area comes out to exactly `2r²`. Not roughly. Not "close enough for a worksheet." **π = 2**, full stop. And it's not a shape I invented for this post — it's the circle of taxicab geometry, the one you'd get if New York cabs ran the universe.

Enough telling you. Here's the thing itself — drag the slider, or just type a π value, and it finds the matching `p` and draws it live. Dip below 2 and the shape folds into thin, spiky stars. Push past it and it rounds out toward a square by the time you hit 4.

<iframe src="https://ristic.in.rs/pi/pi-via-area.html" style="width:100%; max-width:680px; height:760px; border:0; display:block; margin:0 auto;" loading="lazy" title="Interactive explorer: pi defined by area"></iframe>

Here's the twist: area was just one way to look at it. π is just as legitimately defined by circumference instead — and measured that way, the same diamond gives you a completely different number.

<iframe src="https://ristic.in.rs/pi/pi-via-circumference.html" style="width:100%; max-width:680px; height:700px; border:0; display:block; margin:0 auto;" loading="lazy" title="Interactive explorer: pi defined by circumference"></iframe>

Same shapes as before, just measured by walking the edge instead of filling in the inside. The circle is the *only* point where area-π and circumference-π agree — nudge `p` either way and they go their separate ways. "Which one's the real π" is a trick question, same energy as "is a hot dog a sandwich." Depends what you're measuring, and both camps will die mad about it.

The idea keeps going, too. Curve the space itself — draw circles on a sphere — and the ratio bends on its own, no new shape required. Squash a circle into an ellipse, though, and nothing happens: area's still `πab` no matter how hard you pancake it. π shrugs.

My kid got an answer. I got a rabbit hole. Worth it.
