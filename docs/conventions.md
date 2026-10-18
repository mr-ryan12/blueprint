# Engineering Conventions

These conventions apply as application code and tooling are introduced. Technology choices and system boundaries belong to [architecture](architecture.md).

## TypeScript

- Keep strict checking enabled; fix the cause of a type error instead of weakening compiler settings.
- Prefer `unknown` with narrowing for uncertain values and caught errors. Use `any` only for an exceptional, documented reason.
- Do not suppress errors or assert types merely to silence a checker. Explain any necessary exception and its limits.
- Use explicit type imports and generated types where appropriate. Preserve useful inference; annotate public contracts when it improves clarity. Define separate transport shapes when they differ from database models.
- Use runtime validation as described in [security](security.md); a type assertion is not validation.

## Maintainability

- Prefer cohesive modules, clear names, and small changes. Extract shared logic when its meaning is stable; avoid abstractions that only forward calls.
- Match established repository patterns unless the task explicitly changes them. Do not impose arbitrary file-length or function-parameter limits.
- Justify new dependencies in the plan or change description, considering existing capabilities, maintenance, and operational cost.
- Introduce version pins and reproducible installation alongside tooling. Formatting belongs to the configured formatter and EditorConfig once available.

## UI completion

- Use existing components and design tokens after verifying their APIs. Choose visual design to suit the product and preserve consistency.
- Accessibility is part of completion: semantic controls, accessible names, keyboard operation, visible focus, readable contrast, and reduced-motion support.
- Verify responsive layouts and relevant loading, pending, empty, error, and success states.

## Future shared agent and skill content

- Keep shared guidance platform-agnostic; place platform syntax and tool/model configuration in thin adapters.
- Skills should teach reasoning and patterns, with small examples only when necessary. Target 100–300 lines and keep each skill within 500 lines.
- Role definitions and skills should reference canonical project guidance rather than duplicate it.
