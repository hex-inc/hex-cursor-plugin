---
name: hex-to-canvas
description: Carry Hex definitions, caveats, and source links onto a Cursor canvas built from Hex data. Use whenever you put Hex-sourced numbers (from threads, projects, metrics, or query results) into a canvas, or the user asks for a canvas or dashboard from Hex. A plain Hex data question gets a direct answer, not a canvas.
---

# Hex data on a Cursor canvas

A canvas is read later, without the conversation that produced it. Follow Cursor's canvas skill to build it. This skill covers what to carry over from Hex so a reader can trust and trace every number.

Answer a plain data question in chat, with the value, period, and Hex link. If the user wants something shared inside Hex, build it in Hex instead.

## Before you build

- **Read what defines the numbers.** That includes guides, knowledge statements, and metric definitions from `search_context` / `inspect_context`, and the caveats in thread replies. Filters, exclusions, comparability cutoffs, fiscal calendars, and known data issues are caveats the reader needs.
- **Use only exact values Hex returned.** Phrases like "roughly 15% higher" aren't values. Hex returns charts as references (cell IDs or screenshots), not data. If a value is missing, ask Hex for it, or leave it out and say so.
- **Check units and completeness.** Watch for ratios versus percentages, partial periods, and truncated query previews. Don't total a truncated preview.

## On the canvas

- **Put each caveat next to the number it qualifies,** in the stat label or a callout beside the chart, not in a footnote.
- **Keep partial periods off trend charts.** That covers month-to-date, an in-progress week, or an in-progress quarter. Show them in a table or callout labeled partial, with the as-of date.
- **Say whether a number comes from an endorsed definition or an ad hoc query.** If the query differs from the endorsed definition, say how. Don't give it the endorsed metric's name without that qualification.
- **Show the counts behind rates,** e.g. `46.2% (36/78)`. Label any value you computed yourself, such as a delta or a total.
- **Keep local context in its own section.** For commits, flags, or docs, show timing, not causation.
- **End with a sources footer.** Link every Hex thread, project, app, or code project the tools returned (never construct a URL), give the as-of date, and list any local sources.
