---
date: 2024-01-04T10:30:00
title: Fix timeout in ClusterInfoUpdateJob
subtitle: Debugging issues in Elasticsearch
tags:
  - Developer
categories: [Developer]
---

A really small post to document a fix to an issue.

We were getting the following error in the logs of a docker container running elasticsearch that was being used by Graylog.

```
Failed to update shard information for ClusterInfoUpdateJob within 15s timeout
```

The cluster seemed fine with all the indexes green when invoking

```bash
curl http://localhost:9200/_cat/indices
```

In the end the fix was manually close each index (and the closing process would fail with `publication cancelled before committing: timed out after 30s`), so I had to manually retry several times.

```bash
curl -X POST http://localhost:9200/index_name_1/_close
```

Once all indices were closed, I manually reopened them again one by one.

```bash
curl -X POST http://localhost:9200/index_name_1/_open
```

At least this solved my issue.

EDIT: It happened again shortly after. In my case, the indices were too big for the memory allocated. So I configured Graylog to split the index and rotate more often.