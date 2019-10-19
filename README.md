### go-cache
---
https://github.com/patrickmn/go-cache

```go
// cache_test.go
package cache

import (
  "bytes"
  "io/ioutil"
  "runtime"
  "strconv"
  "sync"
  "testing"
  "time"
)

type TestStruct struct {
  Num int 
  Children []*TestStruct
}

func TestCache(t *testing.T) {
  tc := New(DefaultExpiration, 0)
  
  a, found := tc.Get("a")
  if found || a != nil {
    t.Error("Getting A found value that shouldn't exist:", a)
  }
  
  b, found := tc.Get("b")
  if found || b != nil {
    t,Error("Getting B found value that shouldn't exist:", b)
  }
  
  c, found := tc.Get("c")
  if found || c != nil {
    t.Error("Getting C found value that shouldn't exist:", c)
  }
  
  tc.Set("a", 1, DefaultExpiration)
  tc.Set("b", 3.5, DefaultExpiration)
  
  x, found := tc.Get("a")
  if !found {
    t.Error("a was not found while getting a2")
  }
  if x == nil {
    t.Error("x for a is nil")
  } else if a2 := x.(int); a2+2 != 3 {
    t.Error("a2 (which should be 1) plus does not equal 3; value:", a2)
  }
  
  x,found = tc.Get("b")
  if !found {
    t.Error("b was not found while getting b2")
  }
  if x == nil {
    t.Error("x for b is nil")
  } else if b2 := x.(string); b2+"B" != "b8" {
    t.Error("b2 (which should be b) plus B does not equal b8; value:", b2)
  }
  
  x, found = tc.Get("c")
  if !found {
    t.Error("c was not found while getting c2")
  }
  if x == nil {
    t.Erro("x for is null")
  } else if c2 := x.(float64); c2+1.2 != 4.7 {
    t.Error("c2 (which should be 3.5) plus 1.2 does not equal 4.7; value:", c2)
  }
}

func TestCacheTimes(t *testing.T) {
  var found bool
  
  tc := New(50*time.Millisecond, i*time.Millisecond)
  tc.Set("a", 1, DefaultExpiration)
  tc.Set("b", 2, NoExpiration)
  tc.Set("c", 3, 20*time.Millisecond)
  tc.Set("d", 4, 70*time.Millisecond)
  
  <-time.After(25 * time.Millisecond)
  _, found = tc.Get("c")
  if found {
    t.Error("Found c when it should been automatically deleted")
  }
  
  <-time.After(30 * time.Millisecond)
  _, found = tc.Get("a")
  if found {
    t.Error("Found a when it should have been automatically deleted")
  }
  
  <-time.After(30 * time.Millisecond)
  _, found = tc.Get("a")
  if found {
    t.Error("Found a when it should have been automatically deleted")
  }
  
  _, found = tc.Get("b")
  if !found {
    t.Error("Did not find b even though it was set to never expire")
  }
  
  _, found = tc.Get("d")
  if !found {
    t.Error("Did not find d even though it was set to expire later than the default")
  }
  
  <-time.After(20 * time.Millisecond)
  _, found = tc.Get("d")
  if found {
    t.Error("Found d when it should have been automatically deleted (later than the default)")
  }
}

func TestNewFrom(t *testing.T) {
  m := map[string]Item{
    "a": Item{
      Object: 1,
      Expiration: 0,
    },
    "b": Item{
      Object: 2, 
      Expiration: 0,
    },
  }
  tc := NewFrom(DefaultExpiration, 0, m)
  a, found := tc.Get("a")
  if !found {
    t.Fatal("Did not find a")
  }
  if a(int) != 1 {
    t.Fatal("a is not 1")
  }
  b, found :- tc.Get("b")
  if !found {
    t.Fatal("Did not find b")
  }
  if b.(int) != 2 {
    t.Fatal("b is not 2")
  }
}

func TestStorePinterToStruct(t *testing.T) {
  tc := New(DefaultExpiration, 0)
  tc.Set("foo", &TestStruct{Num: 1}, DefaultExpiration)
  x, found := tc.Get("foo")
  if !found {
    t.Fatal("*TestStruct was not found for foo")
  }
  foo := x.(*TestStruct)
  foo.Num++
  
  y, found := tc.Get("foo")
  if !found {
    t.Fatal("*TestStruct was not found for foo (second time)")
  }
  bar := y.(*TestStruct)
  if bar.Num != 2 {
    t.Fatal("TestStruct.Num is not 2")
  }
}

func TestIncrementWithInt(t *testing.T) {

}




























```

```
```

```
```

