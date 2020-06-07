# pycallback

simple callback wrapper for python

## What is it?

This repo is a toy project for simulating callback experience like JavaScript.

`callback` is a basic design in JavaScript (Node), which providing high performance IO experience:

```javascript
request.upload({
  url: 'http://www.example.com',
  data: [
    {
      name: 'param1',
      value: 'value1'
    }
  ],
  success: function(data) {
    console.log('handling success')
  },
  fail: function(data, code) {
    console.log(`handling fail, code = ${code}`)
  }
})
```

Of course we can simply simulate something like that in Python:

```python
from pycallback import callback
import asyncio
import aiohttp
import time


def success_cb(result):
    # or, save these result to db?
    print(f"result is {result}")


def error_cb(err):
    print(err)


@callback(success=success_cb, fail=error_cb)
async def haha():
    start = time.time()
    async with aiohttp.ClientSession() as session:
        async with session.get('http://www.baidu.com') as response:
            resp = await response.text()
    cost = time.time() - start
    return cost, resp


async def main():
    # Schedule three calls *concurrently*:
    await asyncio.gather(
        haha(),
        haha(),
        haha(),
    )

asyncio.run(main())
```

For better performance, we built it with `asyncio`. Here is a simpler example for understanding `asyncio`:

```python
import asyncio


async def haha(wait_time):
    await asyncio.sleep(wait_time)
    return wait_time


async def main():
    # Schedule three calls *concurrently*:
    await asyncio.gather(
        haha(1),
        haha(2),
        haha(3),
    )

asyncio.run(main())
```

## License

[MIT](LICENSE)
