# What's New in IPC

## Nika Layzell - Jun 20, 2019

---

# No more `mIPCOpen`

```c++
if (CanSend()) {
  SendMessage(...);
}
```

---

# Crashes Begone!

 * `SendFoo()` after `ActorDestroy()`.
 * Racing `SendFoo()` with `Send__delete__()`
 * `SendPBarConstructor()` during Shutdown.
 * Repeated `Close()` on a closed channel.

### drop, don't crash

---

# Refcounted Objects

```
using refcounted class nsIPrincipal
    from "mozilla/dom/PermissionMessageUtils.h";
```

```
using refcounted class nsIURI
    from "mozilla/ipc/URIUtils.h";
```

---

# Refcounting
*Coming "Soon"*

```
async refcounted protocol PFoo { ... }
```

---

# Devirtualized Actors

Omit Unnecessary References:
 
```c++
RecvSomeMessage(bool);
```

Use r-value References:

```c++
RecvSomeMessage(nsCString&&);
```

---

# Managed Endpoints

```c++
ManagedEndpoint<PFooParent> ep =
    mgrChild->OpenPFooEndpoint(do_AddRef(child).take());
/* ... */
mgrParent->BindPFooEndpoint(do_AddRef(parent).take(),
                            std::move(ep));
```

---

# Improved Lifecycles

 * No `DeallocPFoo` during a `Recv` callback.
 * No manager `DeallocPBar` before managee.
 * Avoiding nested event loop use-after-frees!

---

# In-Process Actors

 * Main Thread → Main Thread
 * Async Only
 * Uniform API for in-parent & in-child
 * Used by `PWindowGlobal`

---

# Let's Chat!

## Fission → Even More IPC

Let us know about your gripes, ideas, & more!



