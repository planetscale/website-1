---
title: InitShardMaster
weight: 9
---

The InitShardMaster step initializes the quorum by electing a primary and setting up replication for the replicas. Additionally, a database is created to store the data for the keyspace-shard. The default name for the database will be `vt_` followed by the keyspace. In our use case, it would be `vt_commerce`. You can override this default by providing an `init_db_name_override` flag to vttablet. However, all future invocations must continue to supply this parameter.

The command for `InitShardMaster` is as follows:

```text
vtctlclient \
  InitShardMaster \
  -force \
  commerce/0 \
  zone1-100
```

If you have semi-sync enabled and did not set up at least three replicas, InitiShardMaster could hang indefinitely. Even if it succeeds, future operations that perform failovers could cause this shard to go into a deadlocked state.

After this step, visiting the `/debug/status` page on the vttablets should show all the tablets as healthy:

![healthy-tablet](../img/healthy-tablet.png)

{{< warning >}}
`InitShardMaster` is a destructive command that resets all servers by deleting their binlog metadata. It should only be used for initializing a brand new cluster.
{{< /warning >}}

{{< info >}}
`InitShardMaster` will soon be deprecated. This action will be performed automatically by `vtorc` once it is released as production-ready.
{{< /info >}}