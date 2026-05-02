# Theme Test Document

A comprehensive test of markdown rendering and syntax highlighting across languages.

---

## Typography

Regular paragraph text with **bold**, *italic*, ~~strikethrough~~, and `inline code` all in one sentence. You can also combine **_bold italic_** and even `inline code with spaces`.

> This is a blockquote. It can span multiple lines and should render with a left border and muted italic styling.
>
> Second paragraph inside the blockquote.

---

## Lists

### Unordered

- First item
- Second item
  - Nested item
  - Another nested item
    - Deeply nested
- Third item

### Ordered

1. Step one
2. Step two
3. Step three
   1. Sub-step A
   2. Sub-step B

---

## Table

| Language   | Paradigm     | Typing   | Use Case          |
|------------|--------------|----------|-------------------|
| Python     | Multi         | Dynamic  | Scripting, ML     |
| Go         | Concurrent    | Static   | Systems, APIs     |
| TypeScript | OO / FP       | Static   | Web, Node         |
| Kotlin     | OO / FP       | Static   | Android, JVM      |
| Bash       | Imperative    | Dynamic  | Shell automation  |

---

## Code Blocks

### Python

```python
# Fibonacci with memoization
from functools import lru_cache
from typing import Generator

@lru_cache(maxsize=None)
def fib(n: int) -> int:
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

def fib_stream(limit: int) -> Generator[int, None, None]:
    """Yield Fibonacci numbers up to limit."""
    a, b = 0, 1
    while a < limit:
        yield a
        a, b = b, a + b

if __name__ == "__main__":
    numbers = list(fib_stream(1000))
    print(f"Found {len(numbers)} numbers: {numbers}")
```

### Bash

```bash
#!/usr/bin/env bash
set -euo pipefail

# Deploy script — builds and pushes a Docker image
IMAGE="myapp"
TAG="${1:-latest}"
REGISTRY="registry.example.com"

log() {
    echo "[$(date +%H:%M:%S)] $*" >&2
}

build_image() {
    local tag="$1"
    log "Building ${IMAGE}:${tag}..."
    docker build \
        --build-arg BUILD_DATE="$(date -u +%Y-%m-%dT%H:%M:%SZ)" \
        --tag "${REGISTRY}/${IMAGE}:${tag}" \
        .
}

push_image() {
    local tag="$1"
    log "Pushing to ${REGISTRY}..."
    docker push "${REGISTRY}/${IMAGE}:${tag}"
}

build_image "$TAG"
push_image "$TAG"
log "Done."
```

### Go

```go
package main

import (
    "context"
    "fmt"
    "log"
    "net/http"
    "time"
)

type Server struct {
    addr    string
    timeout time.Duration
}

func NewServer(addr string) *Server {
    return &Server{
        addr:    addr,
        timeout: 30 * time.Second,
    }
}

func (s *Server) handler(w http.ResponseWriter, r *http.Request) {
    ctx, cancel := context.WithTimeout(r.Context(), s.timeout)
    defer cancel()

    select {
    case <-ctx.Done():
        http.Error(w, "request timed out", http.StatusGatewayTimeout)
    default:
        fmt.Fprintf(w, "Hello from %s\n", s.addr)
    }
}

func main() {
    srv := NewServer(":8080")
    http.HandleFunc("/", srv.handler)
    log.Printf("Listening on %s", srv.addr)
    log.Fatal(http.ListenAndServe(srv.addr, nil))
}
```

### Kotlin

```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

data class User(
    val id: Long,
    val name: String,
    val email: String,
    val roles: List<String> = emptyList()
)

sealed class Result<out T> {
    data class Success<T>(val value: T) : Result<T>()
    data class Failure(val error: Throwable) : Result<Nothing>()
}

class UserRepository(private val db: Database) {

    fun usersFlow(): Flow<User> = flow {
        db.query("SELECT * FROM users").forEach { row ->
            emit(User(row.getLong("id"), row.getString("name"), row.getString("email")))
        }
    }.flowOn(Dispatchers.IO)

    suspend fun findById(id: Long): Result<User> = runCatching {
        db.findUser(id) ?: error("User $id not found")
    }.fold(
        onSuccess = { Result.Success(it) },
        onFailure = { Result.Failure(it) }
    )
}

fun main() = runBlocking {
    val repo = UserRepository(Database.connect())
    repo.usersFlow()
        .filter { "admin" in it.roles }
        .collect { println("Admin: ${it.name}") }
}
```

### TypeScript

```typescript
import { z } from "zod";

const UserSchema = z.object({
  id: z.string().uuid(),
  name: z.string().min(1),
  email: z.string().email(),
  createdAt: z.coerce.date(),
  roles: z.array(z.enum(["admin", "editor", "viewer"])).default([]),
});

type User = z.infer<typeof UserSchema>;

interface Repository<T, ID> {
  findById(id: ID): Promise<T | null>;
  findAll(): Promise<T[]>;
  save(entity: T): Promise<T>;
  delete(id: ID): Promise<void>;
}

async function withRetry<T>(
  fn: () => Promise<T>,
  attempts = 3,
  delay = 500
): Promise<T> {
  for (let i = 0; i < attempts; i++) {
    try {
      return await fn();
    } catch (err) {
      if (i === attempts - 1) throw err;
      await new Promise((res) => setTimeout(res, delay * 2 ** i));
    }
  }
  throw new Error("unreachable");
}

export class UserService {
  constructor(private readonly repo: Repository<User, string>) {}

  async getUser(id: string): Promise<User> {
    const user = await withRetry(() => this.repo.findById(id));
    if (!user) throw new Error(`User ${id} not found`);
    return user;
  }
}
```

---

## H3 Section

### Nested H3

#### H4 Goes Here

##### H5 Is Smaller

###### H6 Is The Smallest

---

## Inline Elements

Use `kubectl get pods -n default` to list pods. The value of `PI` is approximately `3.14159`. Pass `--verbose` for more output.

A [link to the Glamour repo](https://github.com/charmbracelet/glamour) and some trailing text.

---

*End of test document.*
