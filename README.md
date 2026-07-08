# otter-async

Cooperative async/await runtime (one OS thread per task).

Part of the Otter standard library. Otter is a compiled systems language with no garbage collector and no libc dependency (pthread for threading is the one exception); everything else goes through raw syscalls and DLL imports.

## Install

In your `otter.nest`:

```nest
deps {
  use "async" want "1.0.0"
}
```

Then:

```sh
otter pkg pull
```

## API reference

### `async.spawn(thunk:rawptr, task:rawptr) -> int`

Launches the task thunk on a worker thread. Returns an opaque handle (a control block: { task_ptr, thread_handle }).

Parameters:

- `thunk`: Compiler-generated worker entry (__otter_async_thunk_*)
- `task`: Heap task block carrying args + result + done flag

### `async.poll(handle:int) -> int`

Blocks until the awaited task completes, reclaims the worker thread and control block, and returns the TASK BLOCK address as an int. The compiler's `await` lowering reads the raw result slot (task@8) itself - the slot can hold a tagged bignum, which must not round-trip through a rawptr load - and frees the task block afterwards.

Parameters:

- `handle`: Handle returned by spawn()

Returns: Address of the completed task block

---

## Dependencies

memory, thread.

## License

MIT.
