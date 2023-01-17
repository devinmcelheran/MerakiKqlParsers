# Before Using

The scripts are written in a manner that intends them to be used as functions in a Log Analytics Workspace and **requires** the `StartTime` and `EndTime` parameters to be configured.

These KQL scripts are intended to assist in the parsing of Meraki syslog data sent to a Azure Log Analytics Workspace.

---

### Implementation Details

In contrast to the Meraki/Microsoft provided parser, it has been broken out into different logging areas and makes less use of regex and uses the built-in `parse` and `parse-kv` operators in conjunction with applying a `union` to sub queries to simplify the parsing process and speed it up through the parallelization offered by the `union` operator. As a result, the parser is many times faster.

The `MerakiWirelessEvents.kql` file also takes a different approach to parsing the many key-value pairs. The wireless event logs can contains a few dozen pairs whose presence can depend on event details. Because of this, the `parse-kv` operator wasn't ideal. Instead, it extracts all pairs using regex, packs them into a property bag, and then expands the bag to the columns.