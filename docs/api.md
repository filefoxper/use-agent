# API Reference

## useAgentReducer

This api function is a react hook function, it create a proxy object (`Agent`) by using a model (`OriginAgent`), and you can get latest state from `Agent` object, or use its method to change state.

```typescript
function useAgentReducer<T extends OriginAgent<S>, S>(
    entry: T | { new(): T }, 
    middleWareOrEnv?: MiddleWare | RunEnv, 
    env?: Omit<RunEnv, 'legacy'>
): T
```

* entry - the model class or object.
* middleWareOrEnv - it is an optional param, if you want to use `MiddleWare`, it can be a `MiddleWare`, if you want to set running env without `MiddleWare`, it can be an env config.
* env - if you want to set both `MiddleWare` and running env, you can set an env config here.

Be careful, `LifecycleMiddleWare` can not work with this api directly, so, if you want to use `LifecycleMiddleWares.takeLatest`, you'd better set it with api [useMiddleWare](#useMiddleWare) or [middleWare](https://github.com/filefoxper/agent-reducer/blob/master/documents/en/api/middle_ware.md).

You can find how to config an env object [here](/guides?id=about-run-env) and what is `MiddleWare` [here](https://github.com/filefoxper/agent-reducer/blob/master/documents/en/guides/about_middle_ware.md).

You can see how to use it [here](/tutorial?id=search-page-model).

## useMiddleWare

This api function is a react hook function, it can copy an `Agent` object, and add some different `MiddleWares` on the copy object. So, if you want to use different `MiddleWares` in some special situation on `Agent` method which has its own `MiddleWare`, you can use this api to make a copy `Agent` with different `MiddleWares`. 

The state of copy one keeps equal with the state of `Agent` object you passed in. We suggest you using the state of `Agent` object you passed in.


```typescript
function useMiddleWare<T extends OriginAgent<S>, S>(
    agent: T, 
    ...middleWare: (MiddleWare | LifecycleMiddleWare)[]
):T
```

* agent - `Agent` object which is in a `AgentReducer` function created by [useAgentReducer](/api?id=useagentreducer).
* mdws - `MiddleWares` you want to use in this copy `Agent` object.

You can see how to use it [here](/tutorial?id=use-middleware).

## useAgentSelector

This api function is a react hook, it can be used to extract data from a sharing model state. And only the extracted data change can cause its consumer (react component) rerender.

```typescript
export function useAgentSelector<T extends OriginAgent<S>, S, R>(
  entry: T,
  mapStateCallback: (state: T['state']) => R,
  equalityFn?:(prev: R, current: R) => boolean,
): R
```

* entry - the model instance object.
* mapStateCallback - a callback function for extracting data from model state. The callback param state is a current model state.
* equalityFn - optional param, a callback function to compare the previous extracted data with the current one. If this function returns `true`, `useAgentSelector` do not make its consumer (react component) rerender, no matter if the extracted data changes. Param `prev` is the previous extracted data, and param `current` is the current one.
  
This function returns data extracted from a model state directly.

## useAgentMethods

This api function is a react hook for calling a model methods. It never causes its consumer (react component) rerendering.

```typescript
export function useAgentMethods<T extends OriginAgent<S>, S>(
  entry: T,
  middleWareOrEnv?: MiddleWare | RunEnv,
  env?: Omit<RunEnv, 'legacy'>,
): Omit<T, 'state'>
```

* entry - the model instance object.
* middleWareOrEnv - it is an optional param, if you want to use `MiddleWare`, it can be a `MiddleWare`, if you want to set running env without `MiddleWare`, it can be an env config.
* env - if you want to set both `MiddleWare` and running env, you can set an env config here.

This function returns a model like instance which has an empty state property. It only provides model methods.

## shallowEqual

This api function is a shortcut `equalityFn` callback for using api [useAgentSelector](/api?id=useagentselector). It compares `prev` extracted data and `current` extracted data with a shallow `key-value` equality comparator.

```typescript
function shallowEqual<R>(prev:R, current:R):boolean
```

* prev - one data for comparing with another.
* current - one data for comparing with another.

It returns a `boolean` value. If the two params are equal, it returns `true`, otherwise it returns `false`. 

## ~~useAgent~~

(not recommend)

This api function is an old version abount [useAgentReducer](/api?id=useagentreducer), it is kept for supporting the old features of [agent-reducer](https://www.npmjs.com/package/agent-reducer). And we do not recommend to use it.

## ~~useBranch~~

(not recommend)

This api function is an old version abount [useMiddleWare](/api?id=usemiddleware). And we do not recommend to use it.

## ~~useMiddleActions~~

(not recommend)

This function is a react hook function, it can build an `Agent` using helper. it returns a `MiddleActions` instance which is a proxy object. You can use `method control MiddleWares` on this proxy object. 

We do not recommend to use it, you can see why [here](https://github.com/filefoxper/agent-reducer/blob/master/documents/en/guides/not_recommend.md).

```typescript
function useMiddleActions<T extends OriginAgent<S>, P extends MiddleActions<T, S>, S = any>(
    middleActions: { new(agent: T): P; } | P,
    ...middleWare: (MiddleWare | LifecycleMiddleWare)[]
): P
```

* middleActions - a instance of class extends MiddleActions
* otherParams - start an `Agent` object, and rest with MiddleWares

## ~~AgentProvider~~

(not recommend)

This is a React `Context.Provider` component, it is used to help passing an `Agent` object to a child component deep site in its children tree. 

`agent-reducer@3.2.0` has add a new feature for updating state synchronously by using a same model, and this feature is not dependent on `react Context`, so this api is not recommend now. You can see how to use it [here](/tutorial?id=new-features).

```typescript
interface Props{
  value: T extends OriginAgent,
  children: ReactNodeLike
}
```
* value - the `Agent` object for sharing in `AgentProvider` children.
* children - react nodes

## ~~useAgentContext~~

(not recommend)

This is a react hook function, it picks `Agent` from a nearest [AgentProvider](/api?id=agentprovider) parent component, and update by `Agent` state change.

`agent-reducer@3.2.0` has add a new feature for updating state synchronously by using a same model, and this feature is not dependent on `react Context`, so this api is not recommend now. You can see how to use it [here](/tutorial?id=new-features).

```typescript
function useAgentContext<T extends OriginAgent>(): T
```

## ~~useParent~~

(not recommend)

This api function is an old version abount [useAgentContext](/api?id=useagentcontext), we do not recommend to use it.

  