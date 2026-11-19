---
name: compact-code-style
description: Required house style for all code, YAML and Ansible: compact one-line YAML, regex, minimal code, reuse repo code, vendor research, verify by running, explain in STE100.
---

# Compact Code Style

These rules are requirements. They are not suggestions. Apply all of them to every code, YAML or Ansible task.

## 1. Reuse the repo first

- Before you write code, search the repo. Use `rg` or `grep` for the same task, name, filter, role, template, variable or helper.
- If code that does the job exists, use it. Do not write a second version.
- Use the repo's variable names, file layout, module names and patterns.
- Write new code only when nothing in the repo does the job. Say what you searched for.

## 2. If you do not know, research the vendor

- If you do not know how to do a task, do not guess. Do research first.
- Use the vendor's own sources first: official docs, config guides, API references, release notes, knowledge base, and official GitHub repos or Ansible collections (for example Arista, Fortinet, Eaton, NetBox, Ansible).
- Use the vendor's recommended method, syntax, module and defaults. Match the docs to the version in use.
- Use community sources (forums, blogs) only when the vendor has no answer. Say that the source is not from the vendor.
- In the summary, give the link to each vendor source you used.

## 3. YAML is compact

- Use flow style for short maps and lists: `{path: /etc/x, mode: '0644'}`, `[a, b, c]`.
- One task or one item on one line when it fits in about 120 characters.
- Break a line only when it is longer than that, or when a block (`when`, `loop`) makes it clearer.
- No empty keys. No default values written out. No comments that repeat the code.
- Ansible: use the FQCN (`ansible.builtin.x`). Put the module args in one flow map.

Example:

```yaml
- name: Set NTP server
  ansible.builtin.lineinfile: {path: /etc/chrony.conf, regexp: '^server ', line: 'server {{ ntp_server }} iburst'}
```

## 4. Regex where possible

- Use one regex instead of many string steps, many tasks or a loop.
- Ansible: use `regex_search`, `regex_replace`, `regex_findall`, `lineinfile regexp`, `select('match', ...)`.
- Shell and Python: use one regex instead of chains of `cut`, `split`, `strip`.
- Anchor the regex (`^`, `$`). Use named groups when there is more than one group.
- If a regex is longer than about 60 characters, give it a variable with a clear name.

## 5. Names are clear

- A name tells what the value is: `pdu_outlet_count`, not `n` or `tmp`.
- Use the domain words of the repo. Use one name for one thing.
- Booleans start with `is_`, `has_` or `use_`.

## 6. Code is minimal (Jane Street house style)

- Explicit is better than clever. Simple code that is easy to read is the goal.
- Small functions. One function does one thing.
- No deep nesting. Return early.
- Handle all cases. Fail loud with a clear error message. Do not fail silently.
- Make bad states impossible where the language lets you (types, enums, `assert`, `failed_when`).
- Immutable data where possible. No hidden side effects.
- No dead code, no unused variables, no speculative options, no "for later" code.
- Fewest lines that stay clear. Do not compress until it is hard to read.

## 7. Test and verify, then move on

- Run the real thing. Show the command and the result.
  - YAML: `yamllint` (if present) or a parse check.
  - Ansible: `ansible-playbook --syntax-check`, then `--check --diff` if a target is available. `ansible-lint` if present.
  - Scripts: run once on real or sample input. Show the output.
  - Regex: test it on one line that must match and one line that must not match.
- Write one or two checks for the change. Do not write large test suites unless the user asks.
- If a check fails, fix it and run it again. If you cannot run it, say so and say why.
- When the check passes, stop. Do not add more.

## 8. Explain in STE100 (Simplified Technical English)

All explanations, comments and summaries follow ASD-STE100:

- One instruction in one sentence.
- Instructions: 20 words or fewer. Descriptions: 25 words or fewer.
- Use the imperative for instructions: "Run the playbook."
- Use the active voice. Use simple present or simple past tense.
- Use one word for one meaning. Use approved, simple words ("use", not "utilize"; "start", not "initiate").
- No "-ing" forms as nouns or adjectives when you can avoid them.
- Put each step in a numbered list. Put warnings before the step.
- No filler. Say what changed, what you ran, and the result.

Summary format:

1. What changed (file and line).
2. What repo code you used.
3. What vendor sources you used (links).
4. What command you ran, and the result.
