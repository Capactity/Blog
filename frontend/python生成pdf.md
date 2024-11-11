<img src="/Users/cw/Library/Application Support/typora-user-images/image-20240414110228750.png" alt="image-20240414110228750" style="zoom:50%;" />



同时使用redis缓存当天生成的健康报告，如果用户后面 再次请求之前某一天生成的健康报告，则从redis缓存中读取

1. 如果在同一天内多次请求生成 PDF，则应该从缓存中读取而不是重新生成。
2. 健康报告应该在生成后设置为 7 天后过期    （使用 Redis 的过期键（expire）功能）

​     redis_client.expire(redis_key, expiration_time)



 PDF 文件是二进制数据，不能直接存储在 Redis 中。一种常见的做法是将 PDF 文件转换为字节流（bytes）并存储在 Redis 中

<img src="/Users/cw/Library/Application Support/typora-user-images/image-20240416182019718.png" alt="image-20240416182019718" style="zoom:33%;" />

如果有大量的key需要设置同一时间过期，一般需要注意什么？



如果大量的key过期时间设置的过于集中，到过期的那个时间点，Redis可能会出现短暂的卡顿现象(因为redis是单线程的)。严重的话可能会导致服务器雪崩，所以我们一般在过期时间上加一个随机值，让过期时间尽量分散。