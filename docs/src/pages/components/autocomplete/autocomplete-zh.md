---
title: React Autocomplete（自动补全）组件
components: TextField, Popper, Autocomplete
githubLabel: 'component: Autocomplete'
waiAria: 'https://www.w3.org/TR/wai-aria-practices/#combobox'
---

# Autocomplete 自动补全组件

<p class="description">自动补全是一个普通文本输入框，它通过一组建议的选项来帮助用户输入。</p>

该组件常用于以下两个场景中的单行文本框赋值：

1. 文本框必须取值于某个预设好的，例如：一个位置域必须包含一个有效的位置名称： [组合框](#combo-box)。
2. 文本框也可以是任何值，但最好能够为用户提供可能的选项，譬如搜索框可以提供近似的或者曾搜索过的选项以节省用户时间：[灵活的单文本框](#free-solo)。

此组件旨在改进 “react-select” 和 “downshift” 这两个包。

{{"component": "modules/components/ComponentLinkHeader.js"}}

## Combo box 组合框

必须取值于一个预设的可选数据源。

{{"demo": "pages/components/autocomplete/ComboBox.js"}}

### 选项结构

默认情况下，该组件接受和以下结构相同的选项：

```ts
interface AutocompleteOption {
  label: string;
}
// 或者
type AutocompleteOption = string;
```

例如：

```js
const options = [
  { label: 'The Godfather', id: 1 },
  { label: 'Pulp Fiction', id: 2 },
];
// or
const options = ['The Godfather', 'Pulp Fiction'];
```

然而，你也可以通过提供 `getOptionLabel` 属性来使用不同的结构。

### 练习

下面的每个示例都是自动完成组件的一个功能点的演示。

{{"demo": "pages/components/autocomplete/Playground.js"}}

### 选择一个国家

从248个国家中选择一个。

{{"demo": "pages/components/autocomplete/CountrySelect.js"}}

### 可控的状态

此组件有两种可控的状态：

1. “value” 状态（state）包含了 `value`/`onChange` 两种属性的组合。 这个状态表示用户选择的值，如当按下 <kbd class="key">Enter</kbd> 键时。
2. “input value” 状态（state) 则包含了 `inputValue`/`onInputChange` 两种属性的组合。 这个状态展示了在文本框中显示的值。

> ⚠️ 以上两种状态互不干涉，它们应该被单独控制着。

{{"demo": "pages/components/autocomplete/ControllableStates.js"}}

## Free solo

当将 `freeSolo` 设置为 true 时，用户可以文本框中输入任意值。

### 搜索输入栏

该属性的主要使用方式是创建一个带有搜索建议的 **输入文本框**，例如 Google 搜索 或 react-autowhatever。

{{"demo": "pages/components/autocomplete/FreeSolo.js"}}

### Creatable （可创造性）

如果您打算将此模块用于类似 [组合框](#combo-box) 的体验（一个选择控件元素的增强版），我们则建议如下的设置：

- `selectOnFocus` 帮助用户清除所选值。
- `clearOnBlur` 帮助用户输入一个新的值。
- `handleHomeEndKeys` 使用<kbd class="key">Home</kbd> 和 <kbd class="key">End</kbd> 键在弹出窗口内移动焦点。
- 最后一个选项，例如 `加上 "你的搜索结果"`。

{{"demo": "pages/components/autocomplete/FreeSoloCreateOption.js"}}

您也可以在用户想要加入一个新值的时候显示一个对话框。

{{"demo": "pages/components/autocomplete/FreeSoloCreateOptionDialog.js"}}

## 分组

你可以使用 `groupBy` 属性对选项进行分组。 如果你要这样做，请先确保选项也按照它们分组的相同维度进行排序， 否则你将会注意到重复的标题。

{{"demo": "pages/components/autocomplete/Grouped.js"}}

## 失效的选项

{{"demo": "pages/components/autocomplete/DisabledOptions.js"}}

## `useAutocomplete`

For advanced customization use cases, a headless `useAutocomplete()` hook is exposed. 它接受几乎与 Autocomplete 组件相同的参数，辅以与 JSX 渲染有关的所有参数。 自动完成组件是建立在这个钩子上。

```tsx
import { useAutocomplete } from '@mui/base/AutocompleteUnstyled';
```

The `useAutocomplete` hook is also reexported from @mui/material for convenience and backward compatibility.

```tsx
import useAutocomplete from '@mui/material/useAutocomplete';
```

- 📦  [4.5kB 的压缩包](/size-snapshot)。

{{"demo": "pages/components/autocomplete/UseAutocomplete.js", "defaultCodeOpen": false}}

### 自定义的 hook

{{"demo": "pages/components/autocomplete/CustomizedHook.js"}}

Head to the [customization](#customization) section for an example with the `Autocomplete` component instead of the hook.

## 异步请求

The component supports two different asynchronous use-cases:

- [打开时加载](#load-on-open)：它将等待用户与组件进行交互以加载选项。
- [当你键入内容时进行搜索](#search-as-you-type)：每一次键入都会提交一个新的请求。

### 打开时加载

It displays a progress state as long as the network request is pending.

{{"demo": "pages/components/autocomplete/Asynchronous.js"}}

### 当你键入内容时进行搜索

If your logic is fetching new options on each keystroke and using the current value of the textbox to filter on the server, you may want to consider throttling requests.

Additionally, you will need to disable the built-in filtering of the `Autocomplete` component by overriding the `filterOptions` prop:

```jsx
<Autocomplete filterOptions={(x) => x} />
```

### Google Maps Places

A customized UI for Google Maps Places Autocomplete.

{{"demo": "pages/components/autocomplete/GoogleMaps.js"}}

For this demo, we need to load the [Google Maps JavaScript](https://developers.google.com/maps/documentation/javascript/tutorial) API.

> ⚠️在你开始使用 Google Maps JavaScript API 之前，你必须注册并且创建一个可支付的账户。

## 多个值

Also known as tags, the user is allowed to enter more than one value.

{{"demo": "pages/components/autocomplete/Tags.js"}}

### 固定的选项

In the event that you need to lock certain tag so that they can't be removed in the interface, you can set the chips disabled.

{{"demo": "pages/components/autocomplete/FixedTags.js"}}

### Checkboxes 复选框

{{"demo": "pages/components/autocomplete/CheckboxesTags.js"}}

### 限制标签数量

You can use the `limitTags` prop to limit the number of displayed options when not focused.

{{"demo": "pages/components/autocomplete/LimitTags.js"}}

## 尺寸

想要使用外观看起来比较小的输入框吗？ 试着使用 `size` 属性吧。

{{"demo": "pages/components/autocomplete/Sizes.js"}}

## Customization 个性化

### 自定义输入

The `renderInput` prop allows you to customize the rendered input. The first argument of this render prop contains props that you need to forward. Pay specific attention to the `ref` and `inputProps` keys.

{{"demo": "pages/components/autocomplete/CustomInputAutocomplete.js"}}

### GitHub 标签选择器

This demo reproduces the GitHub's label picker:

{{"demo": "pages/components/autocomplete/GitHubLabel.js"}}

Head to the [Customized hook](#customized-hook) section for a customization example with the `useAutocomplete` hook instead of the component.

## 高亮显示

The following demo relies on [autosuggest-highlight](https://github.com/moroshko/autosuggest-highlight), a small (1 kB) utility for highlighting text in autosuggest and autocomplete components.

{{"demo": "pages/components/autocomplete/Highlights.js"}}

## 自定义筛选

The component exposes a factory to create a filter method that can provided to the `filterOptions` prop. You can use it to change the default option filter behavior.

```js
import { createFilterOptions } from '@mui/material/Autocomplete';
```

### `createFilterOptions(config) => filterOptions`

#### 参数

1. `config` (_object_ [optional]):

- `config.ignoreAccents` (_bool_ [optional]): Defaults to `true`. 移除字母的变音符号。
- `config.ignoreCase` (_bool_ [optional]): Defaults to `true`. 所有字母都小写。
- `config.limit` (*number* [optional]): 默认值为 null。 显示限定数量的建议选项。 例如，如果 `config.limit` 是 `100`,，那么只显示前 `100 个` 匹配的选项。 如果存在很多选项匹配，并且虚拟化设置还没建立成时，这样的限制是非常有效的。
- `config.matchFrom` (_'any' | 'start'_ [optional]): 默认值为 `'any'`。
- `config.stringify` (*func* [optional]): 控制如何将一个选项转换成一个字符串，这样，选项就能够和输入文本的片段相匹配。
- `config.trim` (_bool_ [optional]): Defaults to `false`. 删除尾随空格。

#### 返回结果

`filterOptions`: the returned filter method can be provided directly to the `filterOptions` prop of the `Autocomplete` component, or the parameter of the same name for the hook.

In the following demo, the options need to start with the query prefix:

```jsx
const filterOptions = createFilterOptions({
  matchFrom: 'start',
  stringify: (option) => option.title,
});

<Autocomplete filterOptions={filterOptions} />;
```

{{"demo": "pages/components/autocomplete/Filter.js", "defaultCodeOpen": false}}

### Advanced 进阶

For richer filtering mechanisms, like fuzzy matching, it's recommended to look at [match-sorter](https://github.com/kentcdodds/match-sorter). 就像这样：

```jsx
import { matchSorter } from 'match-sorter';

const filterOptions = (options, { inputValue }) => matchSorter(options, inputValue);

<Autocomplete filterOptions={filterOptions} />;
```

## 可视化

Search within 10,000 randomly generated options. The list is virtualized thanks to [react-window](https://github.com/bvaughn/react-window).

{{"demo": "pages/components/autocomplete/Virtualize.js"}}

## 事件

If you would like to prevent the default key handler behavior, you can set the event's `defaultMuiPrevented` property to `true`:

```jsx
<Autocomplete
  onKeyDown={(event) => {
    if (event.key === 'Enter') {
      // Prevent's default 'Enter' behavior.
      event.defaultMuiPrevented = true;
      // your handler code
    }
  }}
/>
```

## 设计局限

### autocomplete/autofill

Browsers have heuristics to help the user fill in form inputs. However, this can harm the UX of the component.

By default, the component disables the input **autocomplete** feature (remembering what the user has typed for a given field in a previous session) with the `autoComplete="off"` attribute. Google Chrome does not currently support this attribute setting ([Issue 587466](https://bugs.chromium.org/p/chromium/issues/detail?id=587466)). A possible workaround is to remove the `id` to have the component generate a random one.

In addition to remembering past entered values, the browser might also propose **autofill** suggestions (saved login, address, or payment details). In the event you want the avoid autofill, you can try the following:

- 给输入框一个不同的名字，这样不会给浏览器泄露任何可以滥用的信息。 例如：`id="field1"` 而不是 `id="country"`。 若你不填写 id 的话，该组件则会使用一个随机的 id。
- 设置 `autoComplete="new-password"`（当设置此属性时，有些浏览器会建议输入高复杂度的密码）。

  ```jsx
  <TextField
    {...params}
    inputProps={{
      ...params.inputProps,
      autoComplete: 'new-password',
    }}
  />
  ```

Read [the guide on MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Securing_your_site/Turning_off_form_autocompletion) for more details.

### iOS VoiceOver 辅助功能

VoiceOver on iOS Safari doesn't support the `aria-owns` attribute very well. You can work around the issue with the `disablePortal` prop.

### ListboxComponent

If you provide a custom `ListboxComponent` prop, you need to make sure that the intended scroll container has the `role` attribute set to `listbox`. This ensures the correct behavior of the scroll, for example when using the keyboard to navigate.

## 无障碍设计

(WAI-ARIA: https://www.w3.org/TR/wai-aria-practices/#combobox)

We encourage the usage of a label for the textbox. The component implements the WAI-ARIA authoring practices.
