# Elixir `async` and state management

- Use a `Task` if you want to perform a one-off computation or query asynchronously.
- Use an `Agent` if you just need a simple process to keep the state.
- Use a `GenServer` if you need a long-running server process that store states and performs work concurrently.
- Use a dedicated `GenServer` process if you need to serialize access to a shared resource or service used by many concurrent processes.
- Use a `GenServer` process if you need to schedule background work to run on a periodic interval.
- You can start with one and migrate to another later, like from `Agent` to `GenServer`

## `GenServer` callback functions

### `handle_call(message, from, state)`

You want `synchronous` requests sent by the client. So the client is waiting for an **immediate** answer - like an `ack` answer - with the new state.
`from` is usually ignored. It's format is `{pid, tag}`.

The return is typically `{:reply, reply, new_state}`.

### `handle_cast(message, state)`

Handles `asynchronous ` requests sent by the client. The client isn't waiting an acknowledgment.

The return is typically `{:noreply, new_state}`.

### `handle_info(message, state)`

Handles **all other** requests. Useful to handle unknown messages as informational actions.

The return is typically `{:noreply, state}`.

### `init(args)`

When `GenServer.start(__MODULE__, [], name: @process_name)` is called, `[]` is the initial state that will the `init` function will receive.

You can use to customize the initial state passed by the caller.

### `terminate(reason, state)`

A `handle_call` or `handle_cast` function might return `{:stop, reason, new_state}`.
`terminate` handles the process exit routines, like saving the state and closing files.

## Debugging and Tracing

### Check the current state

```elixir
iex> :sys.get_state(pid)
%HttpServer.PledgeServer.State{cache_size: 5, pledges: [{"rodrigo", 15}, {"margot", 25}]}
```

### Trace changes

```elixir
iex> :sys.trace(pid, true)
:ok
iex> HttpServer.PledgeServer.create_pledge("moe", 20)

*DBG* pledge_server got call {create_pledge,<<"moe">>,20} from <0.152.0>
```

### Full status

`:sys.get_status(pid)`
