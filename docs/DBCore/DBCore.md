---
layout: docs
title: 'DBCore'
---

DBCore is a middleware-approach for Dexie that is superior to the [hooks API](../Table/Table.hook('creating')). These are the reasons it is superior to the hooks API:

1. It allows the injector to perform asynchronic actions before forwarding a call.
2. It allows the injector to take actions both before and after the forwarded call.
3. It covers more use cases, such as when a transaction is created, allow custom index proxies etc.

From Dexie 3.0, all *runtime* calls to IndexedDB goes via DBCore. The hooks API is still there and backward compatible with Dexie 2.0, but its internal implementation is now done using a DBCore middleware.

Not all access to IndexedDB go via DBCore still. Upgrade handling and opening the database are performed directly towards the provided IndexedDB implementation which defaults to *global*.indexedDB. In a future version, also these calls may become intercepted by a similar middleware architecture.

## Purpose
* Be able to invoke middlewares
* Be performant and bulk-oriented

## Non-purpose
* Be a totally database-agnostic API

## Definition

```ts
export interface DBCore<TQuery=DBCoreQuery> {
  stack: "dbcore";
  // Transaction and Object Store
  transaction(req: DBCoreTransactionRequest): DBCoreTransaction;

  // Utility methods
  cmp(a: any, b: any) : number;
  readonly MIN_KEY: Key;
  readonly MAX_KEY: Key;
  readonly schema: DBCoreSchema;
  table(name: string): DBCoreTable<TQuery>;
}
```

See [Dexie.use()](../Dexie/Dexie.use())

See also:
* [DBCoreTransactionRequest](DBCoreTransactionRequest)
* [DBCoreTransaction](DBCoreTransaction)
* [DBCoreSchema](DBCoreSchema)
* [DBCoreTable](DBCoreTable)
* [DBCoreQuery](DBCoreQuery)

## Example

* See [Dexie.use()#example](../Dexie/Dexie.use()#example)