# ticketneedle-corpus

Synthetic major-incident ticket corpus for the
[ticketneedle](https://github.com/to-ha/ticketneedle) benchmark.

## What this is

A reproducible corpus of synthetic IT-operations major-incident tickets,
designed for long-context hallucination testing of LLMs. All tickets are
generated locally via Ollama (`llama3.3:70b`) — no real customer or
vendor data, no proprietary tooling. Vendor-name-free and anonymized,
but narratively realistic.

## Sizes

| Size         | Tickets | Approx. tokens | Purpose                                          |
|--------------|---------|----------------|--------------------------------------------------|
| `small_30`   | 30      | ~30 k          | Smoke test, fast iteration                       |
| `medium_80`  | 80      | ~80 k          | Standard benchmark (codeneedle-jquery analogue)  |
| `large_150`  | 150     | ~150 k         | Long-context stress test                         |

## Domain mix (large_150)

Stratified, operations-realistic weighting:

| Domain                     | Tickets |
|----------------------------|---------|
| Network                    | 30      |
| Containers / Kubernetes    | 25      |
| Database                   | 22      |
| Authentication             | 18      |
| Storage                    | 18      |
| Monitoring                 | 18      |
| Platform infrastructure    | 19      |

Smaller sizes scale proportionally with a per-domain floor (≥3 in
`small_30`, ≥6 in `medium_80`).

## Ticket structure

Each ticket is a single Markdown file with these sections:

- Header (ticket ID, date, priority, domain, status)
- Symptom description (3–5 paragraphs)
- Escalation timeline (chronological, anonymized roles and first names)
- Diagnosis steps and hypotheses
- Resolution steps (numbered)
- Post-mortem note

## Needle index

`<size>/index.json` lists each ticket's planted needles:

- `primary.resolution_steps` — multi-line block (codeneedle-analogous)
- `bonus.root_cause` — one-sentence summary
- `bonus.key_command` — one specific command or config string
- `bonus.escalation_path` — ordered list of roles + names
- `bonus.incident_timestamp` — single timestamp

## Hallucination traps

10–20 % of tickets are grouped into similar-symptom clusters with
**different** resolutions. The benchmark uses these to test whether a
model retrieves the correct ticket's resolution rather than mixing
across tickets in the cluster.

## Reproducibility

`generation_seeds.json` records the seed and per-ticket parameters so
the corpus can be regenerated bit-identical. Generator code lives in
[ticketneedle](https://github.com/to-ha/ticketneedle).

## License

License TBD. The corpus is fully synthetic, contains no real customer,
employee, or vendor data, and is published for research and benchmarking
purposes.
