<!--
By default, each component is backed by a `<div>` element. If you were
to look at a rendered component in your developer tools, you would see
a DOM representation that looked something like:
-->

デフォルトでは、各コンポーネントは`<div>`要素によってバッキングされています。
デベロッパーツールでレンダリングされたコンポーネントを見る場合は、次のようなDOM表現があります。

```html
<div id="ember180" class="ember-view">
  <h1>My Component</h1>
</div>
```

<!--
You can customize what type of element Ember generates for your
component, including its attributes and class names, by creating a
subclass of `Component` in your JavaScript.
-->

JavaScriptに`Component`のサブクラスを作成することで、Emberがそのコンポーネントの属性やクラス名など、コンポーネントに対して生成する要素のタイプをカスタマイズすることができます。

<!--
### Customizing the Element
-->

### 要素のカスタマイズ

<!--
To use a tag other than `div`, subclass `Component` and assign it
a `tagName` property. This property can be any valid HTML5 tag name as a
string.
-->

`div`以外のタグを使用するには、`Component`をサブクラス化し、それにtagNameプロパティを割り当てます。
このプロパティは、任意の有効なHTML5タグ名を文字列として使用できます。

```app/components/navigation-bar.js
import Component from '@ember/component';

export default Component.extend({
  tagName: 'nav'
});
```

```app/templates/components/navigation-bar.hbs
<ul>
  <li>{{#link-to "home"}}Home{{/link-to}}</li>
  <li>{{#link-to "about"}}About{{/link-to}}</li>
</ul>
```

<!--
### Customizing the Element's Class
-->

### 要素のクラスのカスタマイズ

<!--
You can specify the class of a component's element at invocation time the same
way you would for a regular HTML element:
-->

通常のHTML要素と同じ方法で、コンポーネントの要素のクラスを呼び出し時に指定できます。

```hbs
{{navigation-bar class="primary"}}
```

<!--
You can also specify which class names are applied to the component's
element by setting its `classNames` property to an array of strings:
-->

`classNames`プロパティを文字列の配列に設定することによって、コンポーネントの要素に適用されるクラス名を指定することもできます。

```app/components/navigation-bar.js
import Component from '@ember/component';

export default Component.extend({
  classNames: ['primary']
});
```

<!--
If you want class names to be determined by properties of the component,
you can use class name bindings. If you bind to a Boolean property, the
class name will be added or removed depending on the value:
-->

コンポーネントのプロパティによってクラス名を決定する場合は、クラス名バインディングを使用できます。
Booleanプロパティにバインドすると、値に応じてクラス名が追加または削除されます。

```app/components/todo-item.js
import Component from '@ember/component';

export default Component.extend({
  classNameBindings: ['isUrgent'],
  isUrgent: true
});
```

<!--
This component would render the following:
-->

このコンポーネントは以下をレンダリングします。

```html
<div class="ember-view is-urgent"></div>
```

<!--
If `isUrgent` is changed to `false`, then the `is-urgent` class name will be removed.
-->

`isUrgent`が`false`に変更された場合、`is-urgent`クラス名は削除されます。

<!--
By default, the name of the Boolean property is dasherized. You can customize the class name
applied by delimiting it with a colon:
-->

デフォルトでは、ブール値のプロパティの名前が収集されます。 コロンで区切って適用するクラス名をカスタマイズできます。

```app/components/todo-item.js
import Component from '@ember/component';

export default Component.extend({
  classNameBindings: ['isUrgent:urgent'],
  isUrgent: true
});
```

<!--
This would render this HTML:
-->

これはこのHTMLをレンダリングします。

```html
<div class="ember-view urgent">
```

<!--
Besides the custom class name for the value being `true`, you can also specify a class name which is used when the value is `false`:
-->

`true`の値のカスタムクラス名に加えて、値が`false`のときに使用されるクラス名を指定することもできます。

```app/components/todo-item.js
import Component from '@ember/component';

export default Component.extend({
  classNameBindings: ['isEnabled:enabled:disabled'],
  isEnabled: false
});
```

<!--
This would render this HTML:
-->

これはこのHTMLをレンダリングします。

```html
<div class="ember-view disabled">
```

<!--
You can also specify a class which should only be added when the property is
`false` by declaring `classNameBindings` like this:
-->

`classNameBindings`を次のように宣言することで、プロパティが`false`の場合にのみ追加するクラスを指定することもできます。

```app/components/todo-item.js
import Component from '@ember/component';

export default Component.extend({
  classNameBindings: ['isEnabled::disabled'],
  isEnabled: false
});
```

<!--
This would render this HTML:
-->

これはこのHTMLをレンダリングします。

```html
<div class="ember-view disabled">
```

<!--
If the `isEnabled` property is set to `true`, no class name is added:
-->

isEnabledプロパティがtrueに設定されている場合、クラス名は追加されません。

```html
<div class="ember-view">
```

<!--
If the bound property's value is a string, that value will be added as a class name without
modification:
-->

バウンドプロパティの値が文字列の場合、その値は変更されずにクラス名として追加されます。

```app/components/todo-item.js
import Component from '@ember/component';

export default Component.extend({
  classNameBindings: ['priority'],
  priority: 'highestPriority'
});
```

<!--
This would render this HTML:
-->

これはこのHTMLをレンダリングします。

```html
<div class="ember-view highestPriority">
```

<!--
### Customizing Attributes
-->

### 属性のカスタマイズ

<!--
You can bind attributes to the DOM element that represents a component
by using `attributeBindings`:
-->

`attributeBindings`を使用して、コンポーネントを表すDOM要素に属性をバインドできます。

```app/components/link-item.js
import Component from '@ember/component';

export default Component.extend({
  tagName: 'a',
  attributeBindings: ['href'],

  href: 'http://emberjs.com'
});
```

<!--
You can also bind these attributes to differently named properties:
-->

これらの属性を異なる名前のプロパティにバインドすることもできます。

```app/components/link-item.js
import Component from '@ember/component';

export default Component.extend({
  tagName: 'a',
  attributeBindings: ['customHref:href'],

  customHref: 'http://emberjs.com'
});
```

<!--
If the attribute is null, it won't be rendered:
-->

属性がnullの場合、レンダリングされません。

```app/components/link-item.js
import Component from '@ember/component';

export default Component.extend({
  tagName: 'span',
  attributeBindings: ['title'],

  title: null,
});
```

<!--
This would render this HTML when no title is passed to the component:
-->

タイトルがコンポーネントに渡されないとき、これはこのHTMLをレンダリングします。

```html
<span class="ember-view">
```

<!--
...and this HTML when a title of "Ember JS" is passed to the component:
-->

そして、 "Ember JS"というタイトルがコンポーネントに渡されたときのこのHTML：

```html
<span class="ember-view" title="Ember JS">
```
