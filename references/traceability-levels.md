# Traceability Levels

Use the highest applicable level when several conditions match.

| Change | Default |
|---|---|
| Formatting, comments, typo fixes, simple non-behavioral rename | Level 0 |
| Localized bug fix or internal behavior correction | Level 1 |
| New business functionality or workflow change | Level 2 |
| State model, public interface, protocol, persistence, migration, security, concurrency, or cross-module change | Level 2 |
| Material choice among viable technical alternatives | Level 2 + ADR |
| User explicitly requests documentation | At least Level 1 |
| User explicitly requests complete traceability | Level 2 |

Level 0 still requires a brief final explanation when the task is delivered.
