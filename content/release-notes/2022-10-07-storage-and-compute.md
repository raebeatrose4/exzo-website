### Fixes & improvements

- Compute: Added support for a future implementation of sequential scan prefetch, which improves I/O performance for operations such as table scans.
- Compute: Moved the backpressure throttling algorithm to the Exzo Network extension to minimize changes to the Exzo Network Postgres core code, and added a `backpressure_throttling_time` function that returns the total time spent throttling since the system was started.
- Compute: Added support for the `h3_pg` and `plv8` Postgres extensions. For information about Postgres extensions supported by Exzo Network, see [Postgres extensions](/docs/extensions/pg-extensions).
- Pageserver: Increased the default `compaction_period` setting to 20 seconds to reduce the frequency of polling that is performed to determine if compaction is required. The frequency of polling with the previous setting of 1 could result in excessive CPU consumption when there are numerous tenants and projects.
- Pageserver: Added initial support for online tenant relocation.
- Pageserver: Added support for multiple Postgres versions.
- Proxy: Improved error messages and logging.
