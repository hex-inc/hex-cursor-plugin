---
name: hex-to-canvas
description: Use ONLY when the user explicitly asks to build a Cursor canvas or dashboard from Hex data. This skill gets concrete, canvas-ready metric values from Hex Threads (search_projects/create_thread/get_thread/continue_thread) and builds a Cursor canvas, optionally blending in local repo, git, or other MCP context. Do NOT use this skill for ordinary Hex data questions, KPI lookups, or analysis where the user only wants an answer and did not ask for a canvas or dashboard.
---

# Hex Thread to Cursor Canvas

Use this skill to turn a Hex Threads analysis into a Cursor canvas. Hex replies often start as narrative summaries, such as "trending up" or "in a 35-50% band", with charts that live inside Hex. A canvas needs exact values embedded inline, so ask for concrete numbers up front, verify them, and only then build the canvas.

## When to use this skill

This skill is opt-in. Only build a canvas when the user explicitly asks for one. Using the Hex MCP or asking a data question is **not** by itself a request for a canvas.

Use this skill when the user clearly wants a canvas or dashboard artifact, for example:

- "Build a canvas from this Hex dashboard."
- "Turn this Hex thread into a dashboard/canvas."
- "Make me a canvas with these KPIs and sources."

Do **not** use this skill, and do not create a canvas, when the user only wants an answer or analysis, for example:

- "What were our top-selling products last quarter?"
- "Pull the latest activation rate from Hex."
- "Do we have a dashboard about churn?"

For those, answer directly or use the `hex-business-analytics-question` skill. If a data question seems like it might benefit from a canvas but the user did not ask for one, answer first and, at most, offer to build a canvas — do not build one unprompted.

## Workflow

1. **Authenticate if needed.** If the Hex MCP server is not authenticated, use Cursor's MCP authentication flow for the Hex server before invoking Hex tools.
2. **Find the right project or dashboard when relevant.** If the user refers to an existing Hex dashboard, report, project, or metric that "lives in Hex", call `search_projects` to locate it. If more than one result is plausible, ask the user to confirm before proceeding.
3. **Gather local context when relevant.** If the canvas should combine Hex data with local information, collect that context before prompting Hex. Useful sources include repository files, `git log`, feature flags, tickets, docs, or other MCP servers.
4. **Create the Hex thread.** Call `create_thread` with a prompt adapted from the canvas-ready template below.
5. **Poll for completion.** Call `get_thread` until the thread is idle or complete. Hex Threads can take several minutes; share the thread URL with the user when useful.
6. **Verify the numbers.** Confirm the response contains exact values, periods, source names, and numerator/denominator counts for rates. If anything is missing or only qualitative, call `continue_thread` for the specific missing values. Do not infer or fabricate.
7. **Build the canvas.** Create one `.canvas.tsx` file using the Cursor canvas APIs. Embed the verified values inline, and cite sources in captions: Hex thread URL, Hex project/dashboard title, source table/model name, as-of date, and any local file paths or commit SHAs used.
8. **Link the source thread.** Always finish the canvas with a footer that links to the Hex thread conversation the canvas was generated from. See [Always link the source Hex thread](#always-link-the-source-hex-thread).

## Canvas-ready prompt template

Adapt this prompt when creating or continuing a thread:

```text
Give me the top KPIs for <topic> as concrete numbers I can put on a dashboard.
For each metric, return:
- Exact current value
- Period covered
- Numerator and denominator for any rate
- Recent-period versus prior-period comparison and delta
- Source table, model, or Hex project used
- As-of date
- Data-coverage caveats, including empty or future periods, timezone, and small samples

Return the results as:
1. A concise list of metric name + value + period.
2. A markdown table with metric, current value, prior value, delta, period,
   numerator, denominator, source, and caveats.
```

Why each clause matters:

- **Exact current value** blocks vague trend language.
- **Period covered** gives every canvas number a defensible time range.
- **Numerator and denominator** make rates auditable, for example `46.2% = 36 / 78`.
- **Recent versus prior comparison** gives a real MoM, QoQ, or other period-over-period stat instead of an eyeballed trend.
- **Source table, model, or Hex project** belongs in the canvas caption.
- **Caveats** prevent incomplete or noisy data from being presented as settled fact.

## Pull from existing Hex dashboards

Hex Threads can read the data behind projects and dashboards that already exist in the workspace. Use this when the user wants the latest values from an existing Hex artifact instead of a new analysis.

1. Call `search_projects` with the dashboard, project, metric, or topic name.
2. Review candidate results using their `title`, `description`, `generated_summary`, `url`, or `appUrl`.
3. Ask the user to choose if multiple candidates are plausible.
4. Name the selected project explicitly in the `create_thread` prompt:

```text
Using the data behind the Hex project "<project title>", give me the latest
values for <metrics>. Return exact current values, the period each covers,
the numerator and denominator behind any rate, the source table/model, caveats,
and the as-of date.
```

5. Treat returned figures as point-in-time. Include the as-of date and Hex project link in the canvas caption.

## Combine with local information

When the canvas should blend Hex data with local repo or external context:

- Collect local context before creating the thread so the Hex prompt can ask for values that line up with the canvas story.
- Keep Hex-sourced values separate from locally derived values in your notes until final assembly.
- Cite local sources with file paths, line references when available, branch names, commit SHAs, or MCP result links.
- If local context changes the interpretation of a Hex metric, explain that in a `Callout` or short narrative card rather than modifying the Hex number.

## Data-coverage caveats to preserve

Carry caveats from Hex into the canvas honestly:

- **Empty or future periods.** Use the latest complete period with data and label it clearly.
- **Timezone.** Note the project timezone, especially for daily, weekly, or monthly windows.
- **Small-sample noise.** Flag low-count periods instead of presenting swings as durable trends.
- **Lower-volume upticks.** A favorable delta on much lower volume should be labeled preliminary.
- **Definition changes.** If the metric definition, table, filter, or cohort changed, include that in the caption or notes.

## Mapping Hex output to canvas components

- Headline metrics -> `Stat` components in a `Grid`.
- Good or bad movement -> `tone="success"` or `tone="danger"` when the direction is unambiguous.
- Two-part splits, such as wins/losses or pass/fail -> donut-style `PieChart`.
- Period-over-period or category comparisons -> `BarChart` with `valueSuffix` and visible values.
- Full metric list with periods and sources -> `Table` with numeric columns right-aligned.
- Narrative recommendations -> short `Card` components and a `Callout tone="info"` takeaway.
- Source and caveat details -> captions or a compact methodology card.
- Source thread link -> a footer `Card` with a `Link` to the Hex thread conversation (always include this).

## Always link the source Hex thread

Every canvas built from a Hex thread must end with a footer that links back to the Hex thread conversation it was generated from. This keeps the canvas auditable: a reviewer can open the original thread to see the full analysis, the exact prompts used, and any follow-up questions.

- Place the link in a dedicated footer at the very bottom of the canvas, after all stats, charts, tables, and narrative cards.
- Use the thread URL returned by `create_thread`/`get_thread` (for example the thread's `url` or `appUrl`). Never fabricate or guess the URL; if you do not have it, call `get_thread` to retrieve it before finishing the canvas.
- Label the link clearly, for example "Generated from Hex thread" or "View the source Hex thread conversation", so its purpose is obvious.
- If the canvas blended multiple Hex threads, link each source thread in the footer.
- If a value or chart came from an existing Hex project or dashboard rather than the thread, still keep the thread link in the footer and add the project/dashboard link alongside it.

A simple footer card works well:

```tsx
<Card>
  <Text>
    Generated from{" "}
    <Link href={hexThreadUrl}>this Hex thread conversation</Link>
    {" "}· as of {asOfDate}
  </Text>
</Card>
```

## Rules

- **Never invent numbers.** If a value is not in the Hex thread response, ask for it with `continue_thread` or omit that canvas element.
- **Always link the source thread.** Every canvas must end with a footer linking to the Hex thread conversation it was generated from. Use the real thread URL; never guess it.
- **Do not build from qualitative summaries alone.** Terms like "up", "down", "strong", "weak", "roughly", or "about" are not canvas-ready unless paired with exact values and periods.
- **Keep caveats visible.** Do not present preliminary, sparse, future-period, or timezone-sensitive data as settled.
- **Use one canvas file.** Build a single `.canvas.tsx` file and import only from `cursor/canvas`.
- **Keep data lineage clear.** Each chart, stat, or table must have enough caption/source context for a reviewer to trace the number back to Hex or local context.

## Expected Tools

The Hex app currently exposes tools such as `search_projects`, `create_thread`, `get_thread`, and `continue_thread`. Canvas construction should use Cursor canvas APIs and follow any available canvas-specific skill or project guidance. Tool availability can vary by workspace permissions and server-side rollout state.

## Common Triggers

- "Build a canvas from this Hex dashboard."
- "Use Hex Agent to get exact KPI values for a Cursor canvas."
- "Pull the latest metrics from Hex and combine them with this branch."
- "Turn this Hex Thread into a dashboard."
- "Make a canvas with Hex numbers, sources, and caveats."
