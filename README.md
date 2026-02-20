# async

Cooperative async runtime for Otter. Provides task spawning and result polling with immediate-completion scheduling.

## API Reference

### `async.spawn(seed:int) -> int`

Create an async task. Encodes the seed value into a handle that can be polled later.

- **Parameters:**
  - `seed` — Integer value to encode as the task result
- **Returns:** Task handle (encoded integer)

```otter
rock handle:int = async.spawn(42);
```

---

### `async.poll(handle:int) -> int`

Retrieve the result from a previously spawned task handle.

- **Parameters:**
  - `handle` — Handle returned by `async.spawn`
- **Returns:** The decoded result value

```otter
rock handle:int = async.spawn(42);
rock result:int = async.poll(handle);  // 42
```

### How It Works

The current scheduler uses immediate completion: `spawn` encodes the value by multiplying by 7 and adding 3, `poll` reverses the encoding. This provides a foundation for future cooperative multitasking.

## Dependencies

None.

## Install

```
otter pkg add async
otter pkg pull
```
