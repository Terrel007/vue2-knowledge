# Vue Render Helper Functions

## Client Side

```javascript
vm._c = function (a, b, c, d) { return createElement(vm, a, b, c, d, false); };

function installRenderHelpers (target) {
  target._o = markOnce;
  target._n = toNumber;
  target._s = toString;
  target._l = renderList;
  target._t = renderSlot;
  target._q = looseEqual;
  target._i = looseIndexOf;
  target._m = renderStatic;
  target._f = resolveFilter;
  target._k = checkKeyCodes;
  target._b = bindObjectProps;
  target._v = createTextVNode;
  target._e = createEmptyVNode;
  target._u = resolveScopedSlots;
  target._g = bindObjectListeners;
}

// 生成render函数
function generate (
  ast,
  options
) {
  var state = new CodegenState(options);
  var code = ast ? genElement(ast, state) : '_c("div")';
  return {
    render: ("with(this){return " + code + "}"),
    staticRenderFns: state.staticRenderFns
  }
}
```
- `vm._c` : `createElement`

- `Vue.prototype._o` : `markOnce`
- `Vue.prototype._n` : `toNumber`
- `Vue.prototype._s` : `toString`
- `Vue.prototype._l` : `renderList`
- `Vue.prototype._t` : `renderSlot`
- `Vue.prototype._q` : `looseEqual`
- `Vue.prototype._i` : `looseIndexOf`
- `Vue.prototype._m` : `renderStatic`
- `Vue.prototype._f` : `resolveFilter`
- `Vue.prototype._k` : `checkKeyCodes`
- `Vue.prototype._b` : `bindObjectProps`
- `Vue.prototype._v` : `createTextVNode`
- `Vue.prototype._e` : `createEmptyVNode`
- `Vue.prototype._u` : `resolveScopedSlots`
- `Vue.prototype._g` : `bindObjectListeners`

## Server Side
```javascript
var ssrHelpers = {
  _ssrEscape: escape,
  _ssrNode: renderStringNode,
  _ssrList: renderStringList,
  _ssrAttr: renderAttr,
  _ssrAttrs: renderAttrs$1,
  _ssrDOMProps: renderDOMProps$1,
  _ssrClass: renderSSRClass,
  _ssrStyle: renderSSRStyle
};

// ssr编译生成render函数
function generate$1 (
  ast,
  options
) {
  var state = new CodegenState(options);
  var code = ast ? genSSRElement(ast, state) : '_c("div")';
  return {
    render: ("with(this){return " + code + "}"),
    staticRenderFns: state.staticRenderFns
  }
}

```
