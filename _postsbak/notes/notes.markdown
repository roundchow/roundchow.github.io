
LightArt for Open Source

## 名词

✦ LightArt

## 开发流程

### 场景一：新的LightArt定制组件

✦ 使用lightart-root实现一个LightArt组件

注意点：引入LightArtSDK、生命周期HookBehavior、定制化交互、埋点Tracker

### 场景二：LightArtSDK的扩展

✦ 深入LightArtSDK

1) platforms

> lightart/platforms/wxapp/entry

> lightart/platforms/wxapp/runtime/index

(1）LightArt.prototype.$dip

(2）LightArt.prototype._initRuntime

> lightart/platforms/wxapp/runtime/ajax

> ajaxMixin(LightArt) -> lightart/platforms/wxapp/runtime/ajax

export function ajaxMixin(LightArt)

> renderMixin(LightArt) -> lightart/platforms/wxapp/runtime/render

export function renderMixin(LightArt)

2) core

> lightart/core/instance/index

> initMixin(LightArt) -> lightart/core/instance/init

（1）export function initMixin(LightArt)

（2）export function initOptions(vm, options)

> eventsMixin(LightArt) -> lightart/core/instance/event

（1）export function initEvents(vm)

（2）export function eventsMixin(LightArt)

> renderMixin(LightArt) -> lightart/core/instance/render

（1）export function initRender(vm)

（2）export function renderMixin(LightArt)

> actionMixin(LightArt) -> lightart/core/instance/action

（1）export function initAction(vm)

（2）export function actionMixin(LightArt)

> countdownMixin(LightArt) -> lightart/core/instance/countdown

（1）export function initCountdown(vm)

（2）export function countdownMixin(LightArt)

> lifecycleMixin(LightArt) -> lightart/core/instance/lifecycle

（1）export function initLifecycle(vm)

（2）export function lifecycleMixin(LightArt)
