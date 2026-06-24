# 🟥 Redis Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Redis (in-memory key-value store) quick reference.

---

## Connect

```bash
redis-cli                        # local connection
redis-cli -h host -p 6379 -a password
ping                             # -> PONG
```

## Keys (general)

```
SET key value
GET key
DEL key
EXISTS key                  # 0 or 1
KEYS user:*                 # pattern match (avoid in prod)
SCAN 0 MATCH user:* COUNT 100
TYPE key
RENAME old new
FLUSHDB                     # wipe current DB
```

## Expiry / TTL (great for caching/sessions)

```
SET session:abc "data" EX 3600   # expire in 3600s
EXPIRE key 60                    # set TTL to 60s
TTL key                          # seconds left (-1 none, -2 gone)
PERSIST key                      # remove expiry
SETEX key 60 "value"             # set + expire in one call
```

## Strings & Counters

```
SET visits 0
INCR visits                 # atomic +1
INCRBY visits 10
DECR visits
APPEND key "more"
MSET a 1 b 2                 # set multiple
MGET a b
```

## Hashes (objects)

```
HSET user:1 name "Alan" age 21
HGET user:1 name
HGETALL user:1
HMGET user:1 name age
HDEL user:1 age
HINCRBY user:1 age 1
HEXISTS user:1 name
```

## Lists (queues / stacks)

```
LPUSH queue a               # push to head
RPUSH queue b               # push to tail
LPOP queue                  # pop head
RPOP queue
LRANGE queue 0 -1           # all elements
LLEN queue
BRPOP queue 5               # blocking pop (5s timeout)
```

## Sets (unique, unordered)

```
SADD tags admin user
SMEMBERS tags
SISMEMBER tags admin        # 0 or 1
SCARD tags                  # count
SREM tags user
SINTER set1 set2            # intersection
SUNION set1 set2            # union
```

## Sorted Sets (leaderboards)

```
ZADD scores 100 alan 90 bob
ZRANGE scores 0 -1 WITHSCORES        # ascending
ZREVRANGE scores 0 2 WITHSCORES      # top 3
ZINCRBY scores 5 alan
ZRANK scores alan                    # position
ZSCORE scores alan
```

## Pub/Sub

```
SUBSCRIBE channel
PUBLISH channel "message"
PSUBSCRIBE news.*
```

## Transactions

```
MULTI                       # start
INCR a
INCR b
EXEC                        # run queued commands atomically
# DISCARD to cancel
```

## Common Use Cases

```
# Cache-aside pattern (pseudo)
val = GET cache:key
if not val:
    val = fetch_from_db()
    SET cache:key val EX 300

# Rate limiting
INCR rate:userid
EXPIRE rate:userid 60   # allow N per minute

# Session store
SETEX session:token 3600 user_json
```

## Node.js (ioredis)

```javascript
const Redis = require("ioredis");
const redis = new Redis();
await redis.set("key", "value", "EX", 60);
const v = await redis.get("key");
await redis.hset("user:1", "name", "Alan");
await redis.incr("visits");
```

---

[🔝 Back to README](../README.md)
