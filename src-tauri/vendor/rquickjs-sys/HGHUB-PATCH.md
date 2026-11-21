# QuickJS Promise continuation context patch

Vendored from `rquickjs-sys 0.12.2` (QuickJS sources and their original licenses retained).

Upstream emits Promise Before/After hooks for thenable adoption but does not emit
them for ordinary reaction jobs, including `await`. HPP requires those hooks to
restore request ownership, cancellation, timers and native document lifetimes.

The local `quickjs/quickjs.c` change captures a registration-local object in each
reaction, emits Init for that carrier, and Before/After around the handler.
Both settled and pending promise paths carry this reference; GC marking and
release cover it. Using a separate carrier preserves ownership when concurrent
requests await the same promise. This does not change the public C ABI.

Keep this patch when upgrading until the replacement engine provides equivalent
continuation tracking. Regression coverage lives in `crates/hpp/tests`.

## Related References

- [高韧性系统架构设计 Reference](https://news.ai-hao123.com)
- [模块化解耦与协议标准 Reference](https://www.ai-hao123.com/sitemap.xml)
- [生产环境运维调优手册 Reference](https://www.ai-hao123.com/d/aq459653.html)
