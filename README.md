### go-cache
---
https://github.com/patrickmn/go-cache

```go
// cache_test.go


func TestCache(t *testing.T) {
  tc := New(DefaultExpiration, 0)
  
  a, found := tc.Get("a")
  if found || a != nil {
    t.Error("Getting A found value that shouldn't exist:", a)
  }
  
  b, found := tc.Get("b")
  if found || b != nil {
  }
  
  
  
}


```

```
```

```
```

