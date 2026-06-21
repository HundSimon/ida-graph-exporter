# Repository Guidelines

## Project Structure & Module Organization

This repository is a small IDAPython-to-HTML exporter.

- `export_ida_graph.py` contains the IDAPython entry point and all graph extraction/data serialization logic.
- `index_template.html` is the browser renderer template. The exporter injects graph JSON in place of `"__DATA__"`.
- `example/` contains checked-in sample output and comparison assets, including `side-by-side.png`.
- `README.md` documents user-facing behavior and known rendering limitations.

Keep generated exports out of the repository unless they are intentionally added as examples.

## Build, Test, and Development Commands

There is no package manager or build pipeline.

- `python -m py_compile export_ida_graph.py` checks Python syntax without needing IDA modules at runtime.
- Run `export_ida_graph.py` inside IDA Pro with a graph view focused to export the current function graph.
- `python -m http.server 8000` can serve exported HTML or files under `example/` for browser inspection.

The HTML template imports Preact and HTM from `https://esm.sh`, so browser validation may require network access.

## Coding Style & Naming Conventions

Use the existing style: Python dataclasses for exported model objects, explicit helper functions for IDA interactions, and readable snake_case names. Keep type hints on new data fields and functions where practical. Use 4-space indentation in Python and preserve the current HTML/CSS/JS indentation style in `index_template.html`.

For JavaScript in the template, prefer small pure helpers and existing naming patterns such as `COLOR_INDICES`, `getColor`, and `parseTokens`.

## Testing Guidelines

No automated test suite currently exists. For Python-only changes, run `python -m py_compile export_ida_graph.py`. For exporter behavior changes, manually test in IDA Pro against a function graph, save the generated HTML, and open it in a browser. For renderer changes, compare against `example/side-by-side.png` or add/update an example when behavior changes materially.

## Commit & Pull Request Guidelines

Recent commits use short, imperative messages, sometimes with a lowercase scope, for example `readme: better example` or `dynamically detect font size, line height`. Follow that style and keep each commit focused.

Pull requests should include a concise summary, testing performed, and screenshots or exported HTML when rendering output changes. Link related issues when available and call out any IDA version or browser-specific behavior.

## Security & Configuration Tips

Generated HTML can include disassembly text, binary hashes, addresses, and names from the analyzed database. Do not commit sensitive exports by accident. Be explicit when adding example artifacts, and prefer sanitized samples.
